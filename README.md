env.java.opts
https://www.cnblogs.com/slankka/p/16131576.html

containerized.taskmanager.env
https://nightlies.apache.org/flink/flink-docs-release-1.17/docs/deployment/config/


javascript:document.getElementById("username").value="cloud";document.getElementsByName("password")[0].value="Gvq-THU-wDU-9hK";document.querySelector(".btn.btn-primary").click();


https://github.com/candrews/log4jdbc-spring-boot-starter
sql statement log



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



https://stackoverflow.com/questions/60742617/why-is-enableoauth2sso-deprecated
https://docs.spring.io/spring-security/reference/servlet/authentication/session-management.html




set flink.partition-discovery.interval-millis=180000;

Create function BehaviorContentUDTF as 'com.flink.udf.JsonParseFuntion';

CREATE TABLE hdc_kafka_behavior_vmall_accesscloud_behavior_id (
id	VARCHAR,
ac	VARCHAR,
createTime	VARCHAR,
ptd	VARCHAR,
contents	VARCHAR
) WITH ('topic' = 'vmall_accesscloud_behavior_id',
'properties.bootstrap.servers' = '10.68.193.53:21005,10.68.192.126:21005,10.68.195.95:21005,10.68.192.177:21005,10.68.192.80:21005',
'properties.group.id' = 'fc395ed65e304caf8c69b349d21ef2ed1_vmall_accesscloud_behavior_id',
'connector' = 'kafka',
'format' = 'json',
'json.ignore-parse-errors' = 'true',
'json.fail-on-missing-field' = 'false',
'scan.startup.mode' = 'earliest-offset');



create view hdc_kafka_behavior_vmall_accesscloud_behavior_id_view as
select
  cast(id as BIGINT) id,
  T.propCode propCode,
  T.propValue propValue,
  T.createTime createTime,
  ac,
  CAST(T.ptd AS bigint) ptd,
  CAST(SUBSTRING(T.pth, 9, 2) AS bigint) as pth
from
  hdc_kafka_behavior_vmall_accesscloud_behavior_id
  LEFT JOIN LATERAL TABLE(BehaviorContentUDTF(contents)) as T(ptd, createTime, propValue, propCode, pth) ON TRUE
where
  propCode is not null
  and (ac not in ('CN', 'CNQX') and ac is not null)
  and T.createTime>='2022-03-22 00:00:00';



CREATE CATALOG hdc_hive WITH (
	 'type' = 'hive',
	 'default-database' = 'hdc_cs',
	 'hive-conf-dir' = '/data01/BigData/data1/FusionInsight/client/Hive/HCatalog/conf'
);




USE CATALOG hdc_hive;
set table.sql-dialect=hive; 



