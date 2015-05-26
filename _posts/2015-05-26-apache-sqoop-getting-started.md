---
layout: post
title: "apache sqoop getting started"
description: ""
category: [apache]
tags: [sqoop, hive]
---
{% include JB/setup %}


### info

1. efficiently transferring bulk data between `hadoop` and `structured datastores` such as relational databases

1. [sqoop.apache.org](https://sqoop.apache.org/)

### install use brew

1. install

            $ brew install sqoop
            ==> Installing dependencies for sqoop: hive, zookeeper
            ==> Installing sqoop dependency: hive
            ==> Downloading https://www.apache.org/dyn/closer.cgi?path=hive/hive-1.1.0/apache-hive-1.1.0-bin.tar.gz
            ==> Best Mirror http://apache.fayea.com/hive/hive-1.1.0/apache-hive-1.1.0-bin.tar.gz
            ######################################################################## 100.0%
            ==> Caveats
            Hadoop must be in your path for hive executable to work.
            After installation, set $HIVE_HOME in your profile:
              export HIVE_HOME=/usr/local/Cellar/hive/1.1.0/libexec

            If you want to use HCatalog with Pig, set $HCAT_HOME in your profile:
              export HCAT_HOME=/usr/local/Cellar/hive/1.1.0/libexec/hcatalog

            You may need to set JAVA_HOME:
              export JAVA_HOME="$(/usr/libexec/java_home)"
            ==> Summary
            🍺  /usr/local/Cellar/hive/1.1.0: 701 files, 99M, built in 4.9 minutes
            ==> Installing sqoop dependency: zookeeper
            ==> Downloading https://homebrew.bintray.com/bottles/zookeeper-3.4.6_1.yosemite.bottle.1.tar.gz
            ######################################################################## 100.0%
            ==> Pouring zookeeper-3.4.6_1.yosemite.bottle.1.tar.gz
            ==> Caveats
            To have launchd start zookeeper at login:
                ln -sfv /usr/local/opt/zookeeper/*.plist ~/Library/LaunchAgents
            Then to load zookeeper now:
                launchctl load ~/Library/LaunchAgents/homebrew.mxcl.zookeeper.plist
            Or, if you don't want/need launchctl, you can just run:
                zkServer start
            ==> Summary
            🍺  /usr/local/Cellar/zookeeper/3.4.6_1: 207 files, 13M
            ==> Installing sqoop
            ==> Downloading http://www.apache.org/dyn/closer.cgi?path=sqoop/1.4.5/sqoop-1.4.5.bin__hadoop-2.0.4-alpha.tar.gz
            ==> Best Mirror http://apache.dataguru.cn/sqoop/1.4.5/sqoop-1.4.5.bin__hadoop-2.0.4-alpha.tar.gz

            curl: (28) Connection timed out after 5005 milliseconds
            Trying a mirror...
            ==> Downloading https://archive.apache.org/dist/sqoop/1.4.5/sqoop-1.4.5.bin__hadoop-2.0.4-alpha.tar.gz
            ######################################################################## 100.0%
            ==> Caveats
            Hadoop, Hive, HBase and ZooKeeper must be installed and configured
            for Sqoop to work.
            ==> Summary
            🍺  /usr/local/Cellar/sqoop/1.4.5: 66 files, 5.8M, built in 48 seconds

1. dependencies

            $ brew deps sqoop
            hadoop
            hbase
            hive
            python
            zookeeper

    1. [hadoop]({% post_url 2015-04-17-apache-hadoop-on-mac-osx-yosemite %})
    1. [hbase]({% post_url 2015-05-26-apache-hbase-getting-started %})
    1. [hive]({% post_url 2015-05-25-apache-hive-getting-started %})
    1. [python]({% post_url 2015-04-03-anaconda-on-mac %})
    1. [zookeeper]({% post_url 2015-05-26-apache-zookeeper-getting-started %})

### usage

1. [import cookbook](https://cwiki.apache.org/confluence/display/SQOOP/Import+Cookbook)

    1. importing a small table from `db2` (using one map process)

            $ sqoop import --driver com.ibm.db2.jcc.DB2Driver --connect jdbc:db2://db2host:50000/DBNAME \
            --username USER --password passw0rd --table MYTABLE -m 1

    1. import from `oracle` to `avro file` (using 6 map processes)

            # Note: User name and table name must be ALL CAPS
            # You'll get exceptions about lack of columns or non-existent table otherwise.

            $ sqoop import --connect "jdbc:oracle:thin:@(description=(address=(protocol=tcp)(host=orahost)(port=1521))(connect_data=(service_name=myser)))" \
            --username SCOTT --password tiger --table MYTABLE -m 6 \
            --as-avrodatafile --warehouse-dir /staging/sqoop/MYTABLE

    1. sqoop import from `mssql`

            $ sqoop import --driver com.microsoft.sqlserver.jdbc.SQLServerDriver \
            --table MYTABLE --connect 'jdbc:sqlserver://MSSQLSeverHost;database=DBNAME;username=USER;password=PASSWORD'

1. [export cookbook](https://cwiki.apache.org/confluence/display/SQOOP/Export+Cookbook)

    1. exporting data from `csv file` to `teradata`

            # note: teradata connector is required.
            $ sqoop export --connect jdbc:teradata://terahost/DATABASE=MYDB \    
            --username myuser --password mypass \ 
            --export-dir /user/hive/warehouse/mydb.db/data_stg \ 
            --table MYTABLE --input-fields-terminated-by ','  
 
    1. merging data into `oracle` with `oraoop`
            
            $ sqoop export -Doraoop.export.merge=true -Doraoop.update.key.extra.columns="period_start_time" \
            -D mapred.map.max.attempts=1 \
            --connect ... --username user --password pass --table table2 \
            --export-dir /user/hive/warehouse/tables/mytable \
            --verbose -m 16 --update-key co_gid
 
    1. exporting a `hive table` stored in a custom schema to `postgresql`

            # sqoop does not have a notion of custom schemas
            # you need to specify the parameter 
            # --schema with a schema name if your table is stored in a different schema
            # please note that the hive schema must have the same name as the postgres one
            # the --schema parameter must be separated from the rest of the parameters
            # with an extra set of dashes (i.e. -- )
            # and the --schema parameter must come last

            $ sqoop export --connect jdbc:postgresql://postgresql.example.com/database \
            --username sqoop --password sqoop \
            --table cities --export-dir cities --schema us
 
    1. exporting `hdfs dir` to `netezza`

            $ sqoop export --direct --connect jdbc:netezza://NetezzaHost:5480/MyTable --username USER \ 
            --password-file <pwd file location> --batch --export-dir <HDFS dir to export> --table NZTable \ 
            --input-fields-terminated-by '\0001' --input-null-string '\\N' --input-null-non-string '\\N’ 