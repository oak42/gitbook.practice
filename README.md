nohup flink run -m yarn-cluster -p 6   -ytm 2g  -yqu QEC -ynm LatestViewProduct -c com.hoperun.submithonor.LatestViewProduct /srv/BigData/data1/bigdata_flink/dap/vmall_dap_cs-1.0-SNAPSHOT.jar 20220320000000000 > /srv/BigData/data1/bigdata_flink/dap/log/LatestViewProduct.log 2>&1 &




--解决code3内存溢出
set hive.auto.convert.join = true;
set hive.auto.convert.join.noconditionaltask = False;
set hive.mapjoin.localtask.max.memory.usage=0.999;

--设置并行执行
SET hive.exec.parallel=true;
SET mapred.job.name=dwr_hdc.hdc_usercount_info_ptm;
set hive.exec.dynamic.partition =true;
set hive.exec.dynamic.partition.mode = nonstrict;
SET hive.exec.max.dynamic.partitions=1000;
set hive.exec.max.dynamic.partitions.pernode=1000;
set mapred.max.split.size = 256000000;
set mapred.max.split.size.per.node = 128000000;
set mapred.max.split.size.per.rack = 128000000;
set hive.merge.mapfiles=true;
set hive.merge.mapredfiles=true;
set hive.merge.size.per.task=128000000;




SET HDC_DBP_LAST_WEEK = concat(year(date_add(${currentDate}, -1)), '0', weekofyear(date_add(${currentDate}, -1)));
-- select ${hiveconf:HDC_DBP_LAST_WEEK}



public class DapBehaviorInfoCopyManager {

    private static final Logger LOGGER = Logger.getLogger(DapBehaviorInfoCopyManager.class);

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();



        //开启Checkpoint
        env.enableCheckpointing(60000, CheckpointingMode.AT_LEAST_ONCE);
        //超时时间设置为30分钟，防止出现写高斯锁表达到默认超时时间20分钟情况
        env.getCheckpointConfig().setCheckpointTimeout(30*60*1000); //30min
        //checkpoint最大失败次数
        env.getCheckpointConfig().setTolerableCheckpointFailureNumber(5);
        //10分钟重启5次，每次失败后等待1分钟
        env.setRestartStrategy(RestartStrategies.failureRateRestart(5,
                org.apache.flink.api.common.time.Time.of(10, TimeUnit.MINUTES),
                org.apache.flink.api.common.time.Time.of(1,TimeUnit.MINUTES)));

        StateBackend fsStateBackend= new FsStateBackend("hdfs:///flink/hdc_checkpoints");
        env.setStateBackend(fsStateBackend);


        env.setStreamTimeCharacteristic(TimeCharacteristic.ProcessingTime);


        String time = null;
        String rec_flag = null;
        String behaviorInputTable = "vmall_oar_cs.behavior";
        String behaviorContentInputTable = "vmall_oar_cs.behavior_content";
        String behaviorTanantTable = "hdc_cs.behavior_tanant";
        String behaviorTanantContentInputTable = "hdc_cs.behavior_content_tanant";
        String groupId="dmp_test_20221219";

        if (args.length >= 1) {
            time = args[0];
        }
        if (args.length >= 2) {
            rec_flag = args[1];
        }
        if(args.length >= 3) {
            groupId = args[2];
        }

        if ("normal".equals(rec_flag)){
            behaviorInputTable = "vmall_oar_cs.behavior";
            behaviorContentInputTable = "vmall_oar_cs.behavior_content";
        }
        if ("recovery".equals(rec_flag)){
            behaviorInputTable = "vmall_oar_cs.behavior_rec";
            behaviorContentInputTable = "vmall_oar_cs.behavior_content_rec";
        }


        Long kafkaStartTime = DateUtil.getLongTimeStamp(time);

        String topicName="vmall_accesscloud_behavior_id";


        DataStream<String> dataSource = env.addSource(KafkaSourceBuilder.buildSourceStartFromTimeStamp(topicName, groupId, kafkaStartTime,PropertiesUtil.getProperty()));


        SplitStream<JSONObject> splitStream = dataSource.map(new MapFunction<String, JSONObject>() {
            @Override
            public JSONObject map(String value) throws Exception {
                JSONObject  jSONObject = null ;
                try {
                    jSONObject = JSON.parseObject(value.replace("\\\\n","").replace("\\\\r","").replace("\\n","").replace("\\r",""));
                }catch (Exception e) {
                    System.out.println("err value:"+ value);
                    return null;
                }
                return jSONObject;
            }
        }).filter(new FilterFunction<JSONObject>() {
            @Override
            public boolean filter(JSONObject value) throws Exception {
                return value != null && ObjectUtils.toString(value.get("ac")).length() <32 && ObjectUtils.toString(value.get("appCo")).length() <32
                        &&  ObjectUtils.toString(value.get("ptd")).length() >= 8  ;
            }
        }).split(new OutputSelector<JSONObject>() {
            @Override
            public Iterable<String> select(JSONObject jsonObject) {
                String ac = jsonObject.getString("ac");
                List<String> output = new ArrayList<String>();
                // ac= CN or CNQX or空写入默认behavior表，其它ac写入behavior_tenant表
                if(StringUtils.isBlank(ac) || StringUtils.equalsIgnoreCase(ac,"CN") || StringUtils.equalsIgnoreCase(ac,"CNQX")) {
                    output.add("behavior");
                } else {
                    output.add("tanant");
                }
                return output;
            }
        });

        //统一输出到rec表
        if (StringUtils.equals(behaviorInputTable,"vmall_oar_cs.behavior_rec")) {
            DataStream<JSONObject> all = splitStream.select("behavior","tanant");
            getBehaviorSinkRow(all,behaviorInputTable);
            getBehaviorContentSinkRow(all,behaviorContentInputTable);
        } else {
            //分别输出到不同的数据表
            DataStream<JSONObject> behavior = splitStream.select("behavior");
            getBehaviorSinkRow(behavior,behaviorInputTable);
            getBehaviorContentSinkRow(behavior,behaviorContentInputTable);
            DataStream<JSONObject> tanant = splitStream.select("tanant");
            getBehaviorSinkRow(tanant,behaviorTanantTable);
            getBehaviorContentSinkRow(tanant,behaviorTanantContentInputTable);
        }


        env.execute();


    }


    private static void getBehaviorSinkRow(DataStream<JSONObject> dataSource,String inputtable) {

        SingleOutputStreamOperator<Tuple2<Integer, BehaviorCopyManage>> map = dataSource.map(new MapFunction<JSONObject, Tuple2<Integer, BehaviorCopyManage>>() {
            BehaviorCopyManage behaviorCopyManage = new BehaviorCopyManage();
            @Override
            public Tuple2<Integer, BehaviorCopyManage> map(JSONObject jsonObject) throws Exception {
                int key = FlinkRebalanceKey.getKeyValueBehavior();
                behaviorCopyManage.setId(ObjectUtils.toString(jsonObject.get("id")));
                behaviorCopyManage.setTid(ObjectUtils.toString(jsonObject.get("tid")));
                behaviorCopyManage.setUserId(ObjectUtils.toString(jsonObject.get("userId")));
                behaviorCopyManage.setUdid(ObjectUtils.toString(jsonObject.get("udid")));
                behaviorCopyManage.setAc(ObjectUtils.toString(jsonObject.get("ac")));
                behaviorCopyManage.setEventTime(String2timeStampFormat(ObjectUtils.toString(jsonObject.get("eventTime"))));
                behaviorCopyManage.setCreateTime(String2timeStampFormat(ObjectUtils.toString(jsonObject.get("createTime"))));
                behaviorCopyManage.setActionCode(ObjectUtils.toString(jsonObject.get("actionCode")));
                behaviorCopyManage.setActionName(ObjectUtils.toString(jsonObject.get("actionName")));
                behaviorCopyManage.setWebsite(ObjectUtils.toString(jsonObject.get("website")));
                behaviorCopyManage.setUrl(ObjectUtils.toString(jsonObject.get("url")));
                behaviorCopyManage.setRefererUrl(ObjectUtils.toString(jsonObject.get("refererUrl")));
                behaviorCopyManage.setCpsid(ObjectUtils.toString(jsonObject.get("cpsid")));
                behaviorCopyManage.setWi(ObjectUtils.toString(jsonObject.get("wi")));
                behaviorCopyManage.setDirect(ObjectUtils.toString(jsonObject.get("direct")));
                behaviorCopyManage.setAppDeviceType(ObjectUtils.toString(jsonObject.get("appDeviceType")));
                behaviorCopyManage.setAppDeviceId(ObjectUtils.toString(jsonObject.get("appDeviceId")));
                behaviorCopyManage.setAppDeviceVersion(ObjectUtils.toString(jsonObject.get("appDeviceVersion")));
                behaviorCopyManage.setAppSource(ObjectUtils.toString(jsonObject.get("appSource")));
                behaviorCopyManage.setAppDm(ObjectUtils.toString(jsonObject.get("appDm")));
                behaviorCopyManage.setAppLn(ObjectUtils.toString(jsonObject.get("appLn")));
                behaviorCopyManage.setAppNn(ObjectUtils.toString(jsonObject.get("appNn")));
                behaviorCopyManage.setAppOuv(ObjectUtils.toString(jsonObject.get("appOuv")));
                behaviorCopyManage.setAppNt(ObjectUtils.toString(jsonObject.get("appNt")));
                behaviorCopyManage.setAppDat(ObjectUtils.toString(jsonObject.get("appDat")));
                behaviorCopyManage.setAppIa(ObjectUtils.toString(jsonObject.get("appIa")));
                behaviorCopyManage.setAppOsv(ObjectUtils.toString(jsonObject.get("appOsv")));
                behaviorCopyManage.setAppWf(ObjectUtils.toString(jsonObject.get("appWf")));
                behaviorCopyManage.setAppCo(ObjectUtils.toString(jsonObject.get("appCo")));
                behaviorCopyManage.setAppSr(ObjectUtils.toString(jsonObject.get("appSr")));
                behaviorCopyManage.setAppOs(ObjectUtils.toString(jsonObject.get("appOs")));
                behaviorCopyManage.setWapFlowSource(ObjectUtils.toString(jsonObject.get("wapFlowSource")));
                behaviorCopyManage.setPtd(Integer.valueOf(ObjectUtils.toString(jsonObject.get("ptd"))));
                behaviorCopyManage.setPth(daytime2dayhour(ObjectUtils.toString(jsonObject.get("createTime"))));
                behaviorCopyManage.setContents(ObjectUtils.toString(jsonObject.get("contents")));
                behaviorCopyManage.setUa(ObjectUtils.toString(jsonObject.get("ua")));
                behaviorCopyManage.setStrategies(ObjectUtils.toString(jsonObject.get("strategies")));
                behaviorCopyManage.setPageId(ObjectUtils.toString(jsonObject.get("pageId")));
                behaviorCopyManage.setEventType(ObjectUtils.toString(jsonObject.get("eventType")));
                behaviorCopyManage.setAppPath(ObjectUtils.toString(jsonObject.get("appPath")));
                behaviorCopyManage.setProvince(ObjectUtils.toString(jsonObject.get("province")));
                behaviorCopyManage.setCity(ObjectUtils.toString(jsonObject.get("city")));
                behaviorCopyManage.setOaId(ObjectUtils.toString(jsonObject.get("oaid")));
                behaviorCopyManage.setWebPushToken(ObjectUtils.toString(jsonObject.get("webPushToken")));
                behaviorCopyManage.setDc(ObjectUtils.toString(jsonObject.get("dc")));
                String allBury = ObjectUtils.toString(jsonObject.get("allBury"));
                allBury = StringUtils.substring(allBury,0,1);
                behaviorCopyManage.setAllBury(NumberUtils.isDigits(allBury)? Integer.valueOf(allBury) : 0);
                behaviorCopyManage.setElementSign(StringUtils.substring(ObjectUtils.toString(jsonObject.get("elementSign")),0,1000));
                String elementType = ObjectUtils.toString(jsonObject.get("elementType"));
                elementType = StringUtils.substring(elementType,0,2);
                behaviorCopyManage.setElementType(NumberUtils.isDigits(elementType)? Integer.valueOf(elementType) : -1);
                behaviorCopyManage.setElementValue(StringUtils.substring(ObjectUtils.toString(jsonObject.get("elementValue")),0,1000));
                behaviorCopyManage.setSubChannel(ObjectUtils.toString(jsonObject.get("subChannel")));
                behaviorCopyManage.setSubChannelId(ObjectUtils.toString(jsonObject.get("subChannelId")));
                behaviorCopyManage.setUuid(ObjectUtils.toString(jsonObject.get("uuid")));
                behaviorCopyManage.setOpenId(ObjectUtils.toString(jsonObject.get("openId")));
                behaviorCopyManage.setSn(ObjectUtils.toString(jsonObject.get("sn")));
                behaviorCopyManage.setUnionId(StringUtils.substring(ObjectUtils.toString(jsonObject.get("unionId")),0,100));
                behaviorCopyManage.setPlatformType(ObjectUtils.toString(jsonObject.get("platformType")));
                behaviorCopyManage.setSinkTime(getCurrentStamp());
//                String content = ObjectUtils.toString(jsonObject.get("contents"));
//                JSONArray contentsNode = JSONArray.parseArray(content);
//                Iterator iterator = contentsNode.iterator();
//                while (iterator.hasNext()) {
//                    JSONObject contentNode = (JSONObject) iterator.next();
//                    Map<String, Object> contentstr = mapFromJson(contentNode.toString());
//                    behaviorCopyManage.setPth(daytime2dayhour(ObjectUtils.toString(contentstr.get("createTime"))));
//                    break;
//                }




                return new Tuple2<Integer, BehaviorCopyManage>(key, behaviorCopyManage);

            }
        });



        SingleOutputStreamOperator<String> aggregate = map.keyBy(new KeySelector<Tuple2<Integer, BehaviorCopyManage>, Integer>() {
            @Override
            public Integer getKey(Tuple2<Integer, BehaviorCopyManage> integerBehaviorCopyManageTuple2) throws Exception {
                return integerBehaviorCopyManageTuple2.f0;
            }
        }).window(TumblingProcessingTimeWindows.of(Time.seconds(2)))
                .aggregate(new AggregateFunction<Tuple2<Integer, BehaviorCopyManage>, StringBuffer, String>() {
                    @Override
                    public StringBuffer createAccumulator() {
                        return new StringBuffer();
                    }

                    @Override
                    public StringBuffer add(Tuple2<Integer, BehaviorCopyManage> integerBehaviorCopyManageTuple2, StringBuffer stringBuffer) {
                        return stringBuffer.append(integerBehaviorCopyManageTuple2.f1.toString()).append("\n");
                    }

                    @Override
                    public String getResult(StringBuffer stringBuffer) {
                        return stringBuffer.toString().replaceAll("\u0000", "").replace("\\", "\\\\");
                    }

                    @Override
                    public StringBuffer merge(StringBuffer stringBuffer, StringBuffer acc1) {
                        return stringBuffer.append(acc1);
                    }
                });

        CopySinkPostgre copySinkPostgre=new CopySinkPostgre(PropertiesUtil.getProperty("dwsUser"),PropertiesUtil.getProperty("dwsPasswd"),PropertiesUtil.getProperty("dwsUrl"),inputtable) ;

        aggregate.addSink(copySinkPostgre);

    }



    private static void getBehaviorContentSinkRow(DataStream<JSONObject> dataStream,String inputtable) {

        SingleOutputStreamOperator<Tuple2<Integer, BehaviorContentCopyManage>> tuple2SingleOutputStreamOperator = dataStream.flatMap(new FlatMapFunction<JSONObject, Tuple2<Integer, BehaviorContentCopyManage>>() {
            BehaviorContentCopyManage behaviorContentCopyManage = new BehaviorContentCopyManage();
            @Override
            public void flatMap(JSONObject jsonObject, Collector<Tuple2<Integer, BehaviorContentCopyManage>> collector) throws Exception {
                String content = ObjectUtils.toString(jsonObject.get("contents"));
                String beCode = ObjectUtils.toString(jsonObject.get("ac"));
                JSONArray contentsNode = JSONArray.parseArray(content);
                if (contentsNode == null) {
                    return;
                }
                Iterator iterator = contentsNode.iterator();
                while (iterator.hasNext()) {
                    JSONObject contentNode = (JSONObject) iterator.next();
                    Map<String, Object> contentstr = mapFromJson(contentNode.toString());

                    behaviorContentCopyManage.setId(ObjectUtils.toString(jsonObject.get("id")));
                    behaviorContentCopyManage.setPropCode(StringUtils.substring(ObjectUtils.toString(contentstr.get("propCode")),0,64));
                    //propValue超长截取
                    behaviorContentCopyManage.setPropValue(StringUtils.substring(ObjectUtils.toString(contentstr.get("propValue")),0,4096));
                    behaviorContentCopyManage.setCreateTime(eventTimeDateFormat(ObjectUtils.toString(contentstr.get("createTime"))));
                    behaviorContentCopyManage.setPtd(Integer.valueOf(ObjectUtils.toString(contentstr.get("ptd"))));
                    behaviorContentCopyManage.setPth(daytime2dayhour(ObjectUtils.toString(jsonObject.get("createTime"))));
                    behaviorContentCopyManage.setBeCode(beCode);
                    collector.collect(new Tuple2<Integer, BehaviorContentCopyManage>(FlinkRebalanceKey.getKeyValueBehavior(), behaviorContentCopyManage));
                }
            }
        });



        SingleOutputStreamOperator<String> aggregate = tuple2SingleOutputStreamOperator.keyBy(new KeySelector<Tuple2<Integer, BehaviorContentCopyManage>, Integer>() {
            @Override
            public Integer getKey(Tuple2<Integer, BehaviorContentCopyManage> integerBehaviorCopyManageTuple2) throws Exception {
                return integerBehaviorCopyManageTuple2.f0;
            }
        }).window(TumblingProcessingTimeWindows.of(Time.seconds(2)))
                .aggregate(new AggregateFunction<Tuple2<Integer, BehaviorContentCopyManage>, StringBuffer, String>() {
                    @Override
                    public StringBuffer createAccumulator() {
                        return new StringBuffer();
                    }

                    @Override
                    public StringBuffer add(Tuple2<Integer, BehaviorContentCopyManage> integerBehaviorCopyManageTuple2, StringBuffer stringBuffer) {
                        return stringBuffer.append(integerBehaviorCopyManageTuple2.f1.toString()).append("\n");
                    }

                    @Override
                    public String getResult(StringBuffer stringBuffer) {
                        return stringBuffer.toString().replaceAll("\u0000", "").replace("\\", "\\\\");
                    }

                    @Override
                    public StringBuffer merge(StringBuffer stringBuffer, StringBuffer acc1) {
                        return stringBuffer.append(acc1);
                    }
                });

        CopySinkPostgre contentCopySinkPostgre=new CopySinkPostgre(PropertiesUtil.getProperty("dwsUser"),PropertiesUtil.getProperty("dwsPasswd"),PropertiesUtil.getProperty("dwsUrl"),inputtable) ;

        aggregate.addSink(contentCopySinkPostgre);


    }




    public static int daytime2dayhour(String time) {
        if (StringUtils.isNotEmpty(time)){
            return Integer.valueOf(time.substring(0,10));
        } else {
            return 2020010100;
        }
    }

    public static Timestamp getCurrentStamp() {
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");
        String current = sdf.format(new Date());
        return Timestamp.valueOf(current);
    }

    public static Timestamp String2timeStampFormat(String time) {
        if (StringUtils.isNotBlank(time)){
            String format = time.substring(0,4)+"-"+time.substring(4,6)+"-"+time.substring(6,8)
                    +" "+time.substring(8,10)+":"+time.substring(10,12)+":"+time.substring(12,14)+"."+time.substring(14);
            return Timestamp.valueOf(format);
        }else{
            return getCurrentStamp();
        }

    };

    public static Map<String,Object> mapFromJson(String paramString){
//        if (paramString != null) {
        ObjectMapper mapper = new ObjectMapper();
        if (null != paramString) {
            paramString = StringUtils.stripToEmpty(paramString);
            try {
                return (Map) mapper.readValue(paramString, Map.class);
            } catch (IOException e) {
                System.out.println("mapFromJson**************error "+ e.getMessage());
                LOGGER.error("mapFromJson**************error",e);
            }
        }
        return null;
    }


    public static String eventTimeDateFormat(String timeStamp) {
        String eventTime = "";
        SimpleDateFormat format = new SimpleDateFormat("yyyyMMddHHmmssSSS");
        Date date = null;
        try {
            date = format.parse(timeStamp);
            SimpleDateFormat newFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
            eventTime = newFormat.format(date);
        } catch (ParseException e) {
            LOGGER.error("format eventTime error " + e);
        }

        return eventTime;
    }




}
