---
layout: post
title: "apache hive getting started"
description: ""
category: [bigdata]
tags: [hive, hadoop, python, mapper]
---
{% include JB/setup %}


### setup

1. have `hadoop` in your path or `export HADOOP_HOME=hadoop-install-dir`

    * check my post [2015-05-25-apache-hive-on-mac-osx-yosemite]({% post_url 2015-05-25-apache-hive-on-mac-osx-yosemite %})

1. create dir in hdfs

            $ hadoop fs -mkdir     /tmp
            $ hadoop fs -mkdir -p  /user/hive/warehouse
            $ hadoop fs -chmod g+w /tmp
            $ hadoop fs -chmod g+w /user/hive/warehouse

1. running hiveserver2 and beeline

    * hiveserver2 has its own cli beeline

    * HiveCLI is now deprecated

            $ bin/hiveserver2

            $ bin/beeline -u jdbc:hive2://$HS2_HOST:$HS_PORT

    * start beeline and hiveserver2 in the same process for testing purpose

            $ bin/beeline -u jdbc:hive2://

1. running hcatalog

            $ hcatalog/sbin/hcat_server.sh

1. running webhcat (templeton)

            $ hcatalog/sbin/webhcat_server.sh

### configuration management overview

1. default configuration `conf/hive-default.xml`

1. change location of configuration directory by setting `HIVE_CONF_DIR`

1. defining/redefining variables `conf/hive-site.xml`

1. log4j configuration `conf/hive-log4j.properties`

1. inherits hadoop configuration

1. manipulate hive configuration

    * editing `hive-site.xml`

    * using set command

    * invoking hive (deprecated), beeline or hiveserver2

            $ bin/hive        --hiveconf x1=y1 --hiveconf x2=y2

            $ bin/hiveserver2 --hiveconf x1=y1 --hiveconf x2=y2

            $ bin/beeline     --hiveconf x1=y1 --hiveconf x2=y2

    * setting `HIVE_OPTS` environment variable

            --hiveconf x1=y1 --hiveconf x2=y2

### hive mr and local-mode

* enable local mode execution

            hive> set mapred.job.tracker=local;
            # `mapred.local.dir` must point to a valid path
            # /tmp/<username>/mapred/local

### metadata store

* metadata is in an embedded derby database

            javax.jdo.option.ConnectionURL
            # default => ./metastore_db

* metastore can be stored in any database that supported by `jpox`

            javax.jdo.option.ConnectionURL
            javax.jdo.option.ConnectionDriverName

* in the future metastore itself can be a standalone server

### simple example use cases

1. create a table with tab-delimited text file format

            create table u_data(
                userid int,
                movieid int,
                rating int,
                unixtime string
            )
            row format delimited
            fields terminated by '\t'
            stored as textfile;

            # 1. <----------------
            # com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException
            # Specified key was too long; max key length is 767 bytes
            mysql> select * from information_schema.schemata s where schema_name='metastore';
            +--------------+-------------+----------------------------+------------------------+----------+
            | CATALOG_NAME | SCHEMA_NAME | DEFAULT_CHARACTER_SET_NAME | DEFAULT_COLLATION_NAME | SQL_PATH |
            +--------------+-------------+----------------------------+------------------------+----------+
            | def          | metastore   | utf8                       | utf8_general_ci        | NULL     |
            +--------------+-------------+----------------------------+------------------------+----------+
            1 row in set (0.00 sec)

            mysql> alter database metastore character set latin1;
            Query OK, 1 row affected (0.00 sec)

            mysql> select * from information_schema.schemata s where schema_name='metastore';
            +--------------+-------------+----------------------------+------------------------+----------+
            | CATALOG_NAME | SCHEMA_NAME | DEFAULT_CHARACTER_SET_NAME | DEFAULT_COLLATION_NAME | SQL_PATH |
            +--------------+-------------+----------------------------+------------------------+----------+
            | def          | metastore   | latin1                     | latin1_swedish_ci      | NULL     |
            +--------------+-------------+----------------------------+------------------------+----------+
            1 row in set (0.00 sec)

            hive> create table employee(emp_id int, emp_name string, emp_title string) row format delimited fields terminated by ',' stored as textfile;
            OK
            Time taken: 0.755 seconds

            mysql> show variables like '%char%';


1. download [movielens 100k](http://files.grouplens.org/datasets/movielens/ml-100k.zip)

            $ wget http://files.grouplens.org/datasets/movielens/ml-100k.zip

            # or

            $ curl --remote-name http://files.grouplens.org/datasets/movielens/ml-100k.zip

            # unzip
            $ unzip ml-100k.zip

1. load `u.data` into the table just created

            hive> load data local inpath '<path>/u.data'
            overwrite into table u_data;

            hive> select count(*) from u_data;

1. complex data analysis on table `u_data`

    * weekday_maapper.py

            import sys
            import datetime

            for line in sys.stdin:
                line = line.strip()
                userid, movieid, rating, unixtime = line.split('\t')
                weekday = datetime.datetime.fromtimestamp(float(unixtime)).isoweekday()
                print '\t'.join([userid, movieid, rating, str(weekday)])

    * use mapper script

            hive> create table u_data_new(
                userid int,
                movieid int,
                rating int,
                weekday int
            )
            row format delimited
            fields terminated by '\t';

            add file weekday_maapper.py

            insert overwrite table u_data_new
            select 
                transform (userid, movieid, rating, unixtime)
                using 'weekday_maapper.py'
                as (userid, movieid, rating, weekday)
            from u_data;

            # 2. <----------------
            hive> insert overwrite table u_data_new select transform (userid, movieid, rating, unixtime) using 'weekday_maapper.py' as (userid, movieid, rating, weekday) from u_data;
            Query ID = hqlgree2_20150601170202_150ee6ee-fa50-49d9-ad27-7158cda130ef
            Total jobs = 3
            Launching Job 1 out of 3
            Number of reduce tasks is set to 0 since there's no reduce operator
            Job running in-process (local Hadoop)
            2015-06-01 17:02:12,361 Stage-1 map = 0%,  reduce = 0%
            Ended Job = job_local524868591_0001 with errors
            Error during job, obtaining debugging information...
            Job Tracking URL: http://localhost:8080/
            FAILED: Execution Error, return code 2 from org.apache.hadoop.hive.ql.exec.mr.MapRedTask
            MapReduce Jobs Launched: 
            Stage-Stage-1:  HDFS Read: 0 HDFS Write: 0 FAIL
            Total MapReduce CPU Time Spent: 0 msec

            # solution: 2. <----------------
            hive> insert overwrite table u_data_new select transform (userid, movieid, rating, unixtime) using 'python /Users/hqlgree2/Documents/hadoop/hive/weekday_mapper.py' as (userid, movieid, rating, weekday) from u_data;
            Query ID = hqlgree2_20150601221515_9686053f-8a88-45e9-b02a-90e0fff696e9
            Total jobs = 3
            Launching Job 1 out of 3
            Number of reduce tasks is set to 0 since there's no reduce operator
            Job running in-process (local Hadoop)
            2015-06-01 22:15:19,293 Stage-1 map = 100%,  reduce = 0%
            Ended Job = job_local526336603_0006
            Stage-4 is selected by condition resolver.
            Stage-3 is filtered out by condition resolver.
            Stage-5 is filtered out by condition resolver.
            Moving data to: hdfs://localhost:9000/user/hive/warehouse/u_data_new/.hive-staging_hive_2015-06-01_22-15-18_053_7587415369814957289-1/-ext-10000
            Loading data to table default.u_data_new
            Table default.u_data_new stats: [numFiles=1, numRows=100000, totalSize=1179173, rawDataSize=1079173]
            MapReduce Jobs Launched: 
            Stage-Stage-1:  HDFS Read: 4134474 HDFS Write: 1179256 SUCCESS
            Total MapReduce CPU Time Spent: 0 msec
            OK
            Time taken: 1.377 seconds

            hive> select weekday, count(*) from u_data_new group by weekday;
            Query ID = hqlgree2_20150601221616_fb0af076-8d19-4788-bd16-383152f1856d
            Total jobs = 1
            Launching Job 1 out of 1
            Number of reduce tasks not specified. Defaulting to jobconf value of: 1
            In order to change the average load for a reducer (in bytes):
              set hive.exec.reducers.bytes.per.reducer=<number>
            In order to limit the maximum number of reducers:
              set hive.exec.reducers.max=<number>
            In order to set a constant number of reducers:
              set mapreduce.job.reduces=<number>
            Job running in-process (local Hadoop)
            2015-06-01 22:16:47,499 Stage-1 map = 100%,  reduce = 100%
            Ended Job = job_local111019165_0007
            MapReduce Jobs Launched: 
            Stage-Stage-1:  HDFS Read: 10627460 HDFS Write: 2358512 SUCCESS
            Total MapReduce CPU Time Spent: 0 msec
            OK
            1   12254
            2   13579
            3   14430
            4   15114
            5   14743
            6   18229
            7   11651
            Time taken: 1.802 seconds, Fetched: 7 row(s)

1. java.io.FileNotFoundException

    * cp hive-env.sh.template hive-env.sh

            hive> select weekday, count(*) from u_data_new group by weekday;
            Query ID = hqlgree2_20150602221313_3ecfb503-ce16-455f-8b48-64f381df9208
            Total jobs = 1
            Launching Job 1 out of 1
            Number of reduce tasks not specified. Estimated from input data size: 1
            In order to change the average load for a reducer (in bytes):
              set hive.exec.reducers.bytes.per.reducer=<number>
            In order to limit the maximum number of reducers:
              set hive.exec.reducers.max=<number>
            In order to set a constant number of reducers:
              set mapreduce.job.reduces=<number>
            java.io.FileNotFoundException: File does not exist: hdfs://localhost:9000/usr/local/hive/libexec/lib/ST4-4.0.4.jar
                at org.apache.hadoop.hdfs.DistributedFileSystem$22.doCall(DistributedFileSystem.java:1309)
                at org.apache.hadoop.hdfs.DistributedFileSystem$22.doCall(DistributedFileSystem.java:1301)
                at org.apache.hadoop.fs.FileSystemLinkResolver.resolve(FileSystemLinkResolver.java:81)
                at org.apache.hadoop.hdfs.DistributedFileSystem.getFileStatus(DistributedFileSystem.java:1301)
                at org.apache.hadoop.mapreduce.filecache.ClientDistributedCacheManager.getFileStatus(ClientDistributedCacheManager.java:288)
                at org.apache.hadoop.mapreduce.filecache.ClientDistributedCacheManager.getFileStatus(ClientDistributedCacheManager.java:224)
                at org.apache.hadoop.mapreduce.filecache.ClientDistributedCacheManager.determineTimestamps(ClientDistributedCacheManager.java:93)
                at org.apache.hadoop.mapreduce.filecache.ClientDistributedCacheManager.determineTimestampsAndCacheVisibilities(ClientDistributedCacheManager.java:57)
                at org.apache.hadoop.mapreduce.JobResourceUploader.uploadFiles(JobResourceUploader.java:179)
                at org.apache.hadoop.mapreduce.JobSubmitter.copyAndConfigureFiles(JobSubmitter.java:98)
                at org.apache.hadoop.mapreduce.JobSubmitter.submitJobInternal(JobSubmitter.java:193)
                at org.apache.hadoop.mapreduce.Job$10.run(Job.java:1290)
                at org.apache.hadoop.mapreduce.Job$10.run(Job.java:1287)
                at java.security.AccessController.doPrivileged(Native Method)
                at javax.security.auth.Subject.doAs(Subject.java:422)
                at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1657)
                at org.apache.hadoop.mapreduce.Job.submit(Job.java:1287)
                at org.apache.hadoop.mapred.JobClient$1.run(JobClient.java:562)
                at org.apache.hadoop.mapred.JobClient$1.run(JobClient.java:557)
                at java.security.AccessController.doPrivileged(Native Method)
                at javax.security.auth.Subject.doAs(Subject.java:422)
                at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1657)
                at org.apache.hadoop.mapred.JobClient.submitJobInternal(JobClient.java:557)
                at org.apache.hadoop.mapred.JobClient.submitJob(JobClient.java:548)
                at org.apache.hadoop.hive.ql.exec.mr.ExecDriver.execute(ExecDriver.java:428)
                at org.apache.hadoop.hive.ql.exec.mr.MapRedTask.execute(MapRedTask.java:137)
                at org.apache.hadoop.hive.ql.exec.Task.executeTask(Task.java:160)
                at org.apache.hadoop.hive.ql.exec.TaskRunner.runSequential(TaskRunner.java:88)
                at org.apache.hadoop.hive.ql.Driver.launchTask(Driver.java:1638)
                at org.apache.hadoop.hive.ql.Driver.execute(Driver.java:1397)
                at org.apache.hadoop.hive.ql.Driver.runInternal(Driver.java:1183)
                at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1049)
                at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1039)
                at org.apache.hadoop.hive.cli.CliDriver.processLocalCmd(CliDriver.java:207)
                at org.apache.hadoop.hive.cli.CliDriver.processCmd(CliDriver.java:159)
                at org.apache.hadoop.hive.cli.CliDriver.processLine(CliDriver.java:370)
                at org.apache.hadoop.hive.cli.CliDriver.executeDriver(CliDriver.java:754)
                at org.apache.hadoop.hive.cli.CliDriver.run(CliDriver.java:675)
                at org.apache.hadoop.hive.cli.CliDriver.main(CliDriver.java:615)
                at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
                at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
                at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
                at java.lang.reflect.Method.invoke(Method.java:497)
                at org.apache.hadoop.util.RunJar.run(RunJar.java:221)
                at org.apache.hadoop.util.RunJar.main(RunJar.java:136)
            Job Submission failed with exception 'java.io.FileNotFoundException(File does not exist: hdfs://localhost:9000/usr/local/hive/libexec/lib/ST4-4.0.4.jar)'
            FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.mr.MapRedTask

    * mv hive-env.sh hive-env.sh.bak

            hive> select weekday, count(*) from u_data_new group by weekday;
            Query ID = hqlgree2_20150602221515_cf6735ad-2974-48fc-9122-b97f95d107f5
            Total jobs = 1
            Launching Job 1 out of 1
            Number of reduce tasks not specified. Estimated from input data size: 1
            In order to change the average load for a reducer (in bytes):
              set hive.exec.reducers.bytes.per.reducer=<number>
            In order to limit the maximum number of reducers:
              set hive.exec.reducers.max=<number>
            In order to set a constant number of reducers:
              set mapreduce.job.reduces=<number>
            Job running in-process (local Hadoop)
            2015-06-02 22:15:35,666 Stage-1 map = 100%,  reduce = 100%
            Ended Job = job_local1524182395_0001
            MapReduce Jobs Launched: 
            Stage-Stage-1:  HDFS Read: 2358346 HDFS Write: 0 SUCCESS
            Total MapReduce CPU Time Spent: 0 msec
            OK
            1   12254
            2   13579
            3   14430
            4   15114
            5   14743
            6   18229
            7   11651
            Time taken: 2.37 seconds, Fetched: 7 row(s)
