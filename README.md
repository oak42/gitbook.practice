https://stackoverflow.com/questions/72084329/wildcard-query-elasticsearch-doesnt-work-with-multi-words-values
Why it is not working on text type field ?
Because wildcard query is the term level query and it don't apply any analyzer at query time. It will consider your entire query as one pattern. You are matching query on text type field which used standard analyzer at indexing time and token your text to multiple terms and index hence it is working for one term and not multiple term.



https://stackoverflow.com/questions/63500197/how-could-flink-pass-environment-variable-to-job-or-cluster
containerized.taskmanager.env.



SET search_path = dwdhdc;
CREATE  TABLE dwd_hdc_user_behavior_d_f (
	platform text,
	action_id bigint,
	action_code character varying,
	action_type character varying,
	action_name character varying,
	ttid text,
	user_id character varying,
	is_first_type character varying(20),
	action_occur_time timestamp without time zone,
	create_time timestamp without time zone,
	website character varying,
	url text,
	page_name character varying(200),
	refer_url text,
	cpsid character varying,
	wi character varying(128),
	direct character varying(20),
	app_device_type character varying(160),
	brief_name character varying(160),
	app_device_id character varying(500),
	app_device_version character varying(32),
	app_dm character varying(100),
	app_ouv character varying(100),
	app_osv character varying(100),
	app_sr character varying(100),
	app_os character varying(500),
	app_source character varying(32),
	page_title character varying,
	url_mark bigint,
	cps_source_name character varying,
	channel_name character varying,
	session_id text,
	page_duration bigint,
	url_login character varying,
	url_exit character varying,
	session_start_time timestamp without time zone,
	session_end_time timestamp without time zone,
	session_duration bigint,
	pt_w bigint,
	pt_m bigint,
	pt_h bigint,
	be_code character varying(500),
	create_by character varying(20),
	latest_modify_by character varying(20),
	pt_d bigint,
	app_device_type_new character varying,
	series_name character varying(200),
	app_path character varying,
	province character varying(50),
	city character varying(50),
	split_url character varying(4096)
)
WITH (orientation=column, compression=middle)
DISTRIBUTE BY HASH(action_id)
TO GROUP group_version1
PARTITION BY RANGE (pt_d)
(
	 PARTITION p20230612 VALUES LESS THAN (20230612::bigint) TABLESPACE pg_default,
	 PARTITION p20230613 VALUES LESS THAN (20230613::bigint) TABLESPACE pg_default,
	 PARTITION p20230614 VALUES LESS THAN (20230614::bigint) TABLESPACE pg_default,
	 PARTITION p20230615 VALUES LESS THAN (20230615::bigint) TABLESPACE pg_default,
	 PARTITION p20230616 VALUES LESS THAN (20230616::bigint) TABLESPACE pg_default
)
ENABLE ROW MOVEMENT;


SET search_path = dwdhdc;
CREATE  TABLE dwd_hdc_user_behavior_content_d_f (
	be_code character varying,
	behavior_id character varying,
	prop_code character varying,
	prop_value character varying,
	create_time character varying,
	pt_m bigint,
	pt_w bigint,
	pt_h bigint,
	create_by character varying,
	latest_modify_by character varying,
	pt_d bigint
)
WITH (orientation=column, compression=middle)
DISTRIBUTE BY HASH(behavior_id)
TO GROUP group_version1
PARTITION BY RANGE (pt_d)
(
	 PARTITION p20230610 VALUES LESS THAN (20230610::bigint) TABLESPACE pg_default,
	 PARTITION p20230611 VALUES LESS THAN (20230611::bigint) TABLESPACE pg_default,
	 PARTITION p20230612 VALUES LESS THAN (20230612::bigint) TABLESPACE pg_default,
	 PARTITION p20230613 VALUES LESS THAN (20230613::bigint) TABLESPACE pg_default,
	 PARTITION p20230614 VALUES LESS THAN (20230614::bigint) TABLESPACE pg_default,
	 PARTITION p20230615 VALUES LESS THAN (20230615::bigint) TABLESPACE pg_default,
	 PARTITION p20230616 VALUES LESS THAN (20230616::bigint) TABLESPACE pg_default
)
ENABLE ROW MOVEMENT;



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



