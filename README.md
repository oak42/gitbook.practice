Quick start
It is recommended to install docsify-cli globally, which helps initializing and previewing the website locally.

npm i docsify-cli -g

Initialize
If you want to write the documentation in the ./docs subdirectory, you can use the init command.

docsify init ./docs

Writing content
After the init is complete, you can see the file list in the ./docs subdirectory.

index.html as the entry file
README.md as the home page

.nojekyll prevents GitHub Pages from ignoring files that begin with an underscore

You can easily update the documentation in ./docs/README.md, of course you can add more pages.

Preview your site
Run the local server with docsify serve. You can preview your site in your browser on http://localhost:3000.

docsify serve docs

For more use cases of docsify-cli, head over to the docsify-cli documentation.




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
