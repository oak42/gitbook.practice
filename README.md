## initial GitBook practicing




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
