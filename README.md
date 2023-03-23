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
