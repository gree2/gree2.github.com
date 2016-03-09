---
layout: post
title: "sqoop with transwarp data hub"
description: ""
category: [bigdata]
tags: [sqoop, transwarp, tdh]
---
{% include JB/setup %}


### steps

1. topology

            +--------------+ +--------------+ +--------------+
            |              | |              | |              |
            |     node1    | |     node2    | |     node3    |
            |              | |              | |              |
            +--------------+ +--------------+ +--------------+

1. [download sqoop-1.4.6](http://www.apache.org/dyn/closer.lua/sqoop/1.4.6)

1. download [mysql]({% post_url 2015-05-26-apache-sqoop-getting-started %}) and [mssql]({% post_url 2015-05-27-apache-sqoop-import-export-demo %}) connector


    1. [installing the jdbc drivers](http://www.cloudera.com/content/cloudera/en/documentation/cdh4/latest/CDH4-Installation-Guide/cdh4ig_topic_13_7.html) and check my post [2015-05-25-apache-hive-on-mac-osx-yosemite]({% post_url 2015-05-25-apache-hive-on-mac-osx-yosemite %}) and [connectors and drivers](https://blogs.apache.org/sqoop/entry/connectors_and_drivers_in_the) and [documentation](http://sqoop.apache.org/docs/1.4.0-incubating/SqoopUserGuide.html#id1763114)

            $ curl -L 'http://www.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.35.tar.gz/from/http://mysql.he.net/' | tar xz

    1. [download and config sqljdbc](http://hortonworks.com/hadoop-tutorial/import-microsoft-sql-server-hortonworks-sandbox-using-sqoop/)

            $ curl -L 'http://download.microsoft.com/download/0/2/A/02AAE597-3865-456C-AE7F-613F99F850A8/sqljdbc_4.0.2206.100_enu.tar.gz' | tar xz

1. copy sqoop to node2

            $ scp sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz root@node2:/root/

1. installing sqoop

    1. ssh to node2

            # on mac
            $ ssh root@node2

    1. unzip sqoop

            # on node2
            $ tar -zxf sqoop-1.4.6*.gz

    1. rename folder

            # on node2
            $ mv sqoop-1.4.6.bin__hadoop-2.0.4-alpha sqoop1

1. installing connectors

    1. mysql connector

            # on mac
            $ cd mysql-connector-java-5.1.35
            $ scp mysql-connector-java-5.1.35-bin.jar root@node2:/root/sqoop1/lib

    1. mssql connector

            # on mac
            $ cd sqljdbc_4.0/enu
            $ cp sqljdbc4.jar root@node2:/root/sqoop1/lib

1. config sqoop

    1. check `/usr/lib` on node2

            # on node2
            $ ls /usr/lib/
            anaconda-runtime  hbase       java-ext     rpm
            ConsoleKit        hive        jvm          ruby
            cups              java        jvm-commmon  scala
            deploy            java-1.3.1  jvm-exports  sendmail
            games             java-1.4.0  jvm-private  sendmail.postfix
            gcc               java-1.4.1  locale       sparkR
            hadoop            java-1.4.2  lsb          transwarp
            hadoop-hdfs       java-1.5.0  ngmr         transwarp-manager
            hadoop-mapreduce  java-1.6.0  ngmr-shell   yum-plugins
            hadoop-yarn       java-1.7.0  python2.6    zookeeper

    1. config `sqoop-env.sh`

            $ nano sqoop-env.sh
            # Licensed to the Apache Software Foundation (ASF) under one or more
            # contributor license agreements.  See the NOTICE file distributed with
            # this work for additional information regarding copyright ownership.
            # The ASF licenses this file to You under the Apache License, Version 2.0
            # (the "License"); you may not use this file except in compliance with
            # the License.  You may obtain a copy of the License at
            #
            #     http://www.apache.org/licenses/LICENSE-2.0
            #
            # Unless required by applicable law or agreed to in writing, software
            # distributed under the License is distributed on an "AS IS" BASIS,
            # WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
            # See the License for the specific language governing permissions and
            # limitations under the License.

            # included in all the hadoop scripts with source command
            # should not be executable directly
            # also should not be passed any arguments, since we need original $*

            # Set Hadoop-specific environment variables here.

            #Set path to where bin/hadoop is available
            export HADOOP_COMMON_HOME=/usr/lib/hadoop

            #Set path to where hadoop-*-core.jar is available
            export HADOOP_MAPRED_HOME=/usr/lib/hadoop-mapreduce

            #set the path to where bin/hbase is available
            export HBASE_HOME=/usr/lib/hbase

            #Set the path to where bin/hive is available
            export HIVE_HOME=/usr/lib/hive

            #Set the path for where zookeper config dir is
            export ZOOCFGDIR=/usr/lib/zookeeper/conf

1. import data to hdfs

    1. mssql to hdfs

            $ ./sqoop import-all-tables --connect 'jdbc:sqlserver://mssqlhost;database=mydb;username=myname;password=mypass' -m 1

    1. mysql to hdfs

            $ ./sqoop import --connect jdbc:mysql://mysqlhost/mydb --username myname --password mypass --table mytable -m 1

1. import data to hive

    1. single command

            # on node2
            $ ./sqoop import --connect 'jdbc:sqlserver://mssqlhost;database=mydb;username=myname;password=mypass' \
            --table employee --hive-table emps --hive-import

    1. permission denied

            2015-09-06 10:44:18,348 INFO hive.HiveImport: FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. MetaException(message:Got exception: org.apache.hadoop.security.AccessControlException Permission denied: user=root, access=EXECUTE, inode="/inceptorsql1/user/hive/warehouse":hive:hadoop:drwx------

    1. fix permission denied

            # on node2
            $ sudo -u hdfs hdfs dfs -chmod 777 /inceptorsql1/user/hive/warehouse

    1. check data in hive

            # on node2
            $ hive
            transwarp> show databases;
            OK
            default
            Time taken: 3.291 seconds, Fetched: 1 row(s)
            transwarp> use default;
            OK
            Time taken: 0.046 seconds
            transwarp> show tables;
            OK
            gsswdjb
            Time taken: 0.059 seconds, Fetched: 1 row(s)

            transwarp> select * from gsswdjb limit 10;
            OK
            ...
            data
            ...

    1. tableau

            # 1. install tableau

            # 2. install hortonworks hive odbc driver

            # 3. `hortonworks hadoop hive`
            # ip:node2 port:10000

    1. data columns with `new line` characters

            # on node2
            # import 160000 records from sqlserver
            # but got 320000+ records in hive

            # fixed
            $ ./sqoop import --connect 'jdbc:sqlserver://sqlhost;database=mydb;username=myuser;password=mypass' \
            --query "select aa, bb, cc, REPLACE(REPLACE(dd, CHAR(13), '{13}'), CHAR(10), '{10}') dd from mytable where \$CONDITIONS" \
            --hive-table mytable --hive-import  --target-dir /inceptorsql1/user/hive/warehouse/mytable -m 1
