---
layout: post
title: "apache hive getting started"
description: ""
category: [apache]
tags: [hive, hadoop]
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

            # 2. 
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

            create table u_data_new(
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

            # 1. <----------------
            hive> insert overwrite table u_data_new select transform (userid, movieid, rating, unixtime) using 'weekday_maapper.py'as (userid, movieid, rating, weekday) from u_data;
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

            select weekday, count(*)
            from u_data_new
            group by weekday;