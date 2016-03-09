---
layout: post
title: "apache hbase getting started"
description: ""
category: [bigdata]
tags: [hbase, hadoop]
---
{% include JB/setup %}

### info

### install use brew

1. 404 Not Found

    * problem

            $ brew install hbase
            ==> Downloading https://www.apache.org/dyn/closer.cgi?path=hbase/hbase-1.0.0/hbase-1.0.0-bin.tar.gz
            ==> Best Mirror http://mirrors.cnnic.cn/apache/hbase/hbase-1.0.0/hbase-1.0.0-bin.tar.gz

            curl: (22) The requested URL returned error: 404 Not Found
            Error: Failed to download resource "hbase"
            Download failed: http://mirrors.cnnic.cn/apache/hbase/hbase-1.0.0/hbase-1.0.0-bin.tar.gz

    * [solution](http://stackoverflow.com/questions/12757694/brew-install-maven-404-error)

            $ brew update
            Updated Homebrew from 3cebc21b to e8cdcc69.
            ==> New Formulae
            ...
            ==> Updated Formulae
            ...
            ==> Deleted Formulae
            ...

1. success

    * install

            $ brew install hbase
            ==> Installing hbase dependency: hadoop
            ==> Downloading https://www.apache.org/dyn/closer.cgi?path=hadoop/common/hadoop-2.7.0/hadoop-2.7.0.tar.gz
            ==> Best Mirror http://mirrors.cnnic.cn/apache/hadoop/common/hadoop-2.7.0/hadoop-2.7.0.tar.gz
            ######################################################################## 100.0%
            ==> Caveats
            In Hadoop's config file:
              /usr/local/Cellar/hadoop/2.7.0/libexec/etc/hadoop/hadoop-env.sh,
              /usr/local/Cellar/hadoop/2.7.0/libexec/etc/hadoop/mapred-env.sh and
              /usr/local/Cellar/hadoop/2.7.0/libexec/etc/hadoop/yarn-env.sh
            $JAVA_HOME has been set to be the output of:
              /usr/libexec/java_home
            ==> Summary
            🍺  /usr/local/Cellar/hadoop/2.7.0: 6297 files, 324M, built in 31.6 minutes
            ==> Installing hbase
            ==> Downloading https://www.apache.org/dyn/closer.cgi?path=hbase/hbase-1.0.1/hbase-1.0.1-bin.tar.gz
            ==> Best Mirror http://mirror.bit.edu.cn/apache/hbase/hbase-1.0.1/hbase-1.0.1-bin.tar.gz

            curl: (22) The requested URL returned error: 404 Not Found
            Trying a mirror...
            ==> Downloading https://archive.apache.org/dist/hbase/hbase-1.0.1/hbase-1.0.1-bin.tar.gz
            ######################################################################## 100.0%
            ==> Caveats
            You must edit the configs in:
              /usr/local/Cellar/hbase/1.0.1/libexec/conf
            to reflect your environment.

            For more details:
              http://wiki.apache.org/hadoop/Hbase
            ==> Summary
            🍺  /usr/local/Cellar/hbase/1.0.1: 10810 files, 372M, built in 77.8 minutes

    * config hadoop check my post [2015-04-17-apache-hadoop-on-mac-osx-yosemite]({% post_url 2015-04-17-apache-hadoop-on-mac-osx-yosemite %})

### [config](http://freddy.cellcore.org/post/52568231952/hadoop-hbase-on-osx-10-8-mountain-lion)

1. standalone hbase

    1. configure hbase `hbase-site.xml`

            <configuration>
                <property>
                    <name>hbase.rootdir</name>
                    <value>file:///home/hqlgree2/hbase</value>
                </property>
                <property>
                    <name>hbase.zookeeper.property.dataDir</name>
                    <value>/home/hqlgree2/zookeeper</value>
                </property>
            </configuration>

    1. start hbase

            $ start-hbase.sh

            # in standalone mode
            # hbase runs all daemons within a single jvm
            # HMaster, a single HRegionServer
            # and zookeeper daemon

            # if get an error indicating java is not installed
            $ pico hbase-env.sh
            export JAVA_HOME="$(/usr/libexec/java_home)"

    1. connect to hbase

            $ hbase shell
            ...
            hbase(main):001:0>

    1. display hbase shell help text

            hbase(main):001:0> help

    1. create a table

            hbase(main):001:0> create 'test', 'cf'
            0 row(s) in 60.4540 seconds

            => Hbase::Table - test

    1. list information of table

            hbase(main):001:0> list 'test'
            TABLE
            test                                                                                        
            1 row(s) in 0.0090 seconds

            => ["test"]

    1. put data into your table

            hbase(main):001:0> put 'test', 'row1', 'cf:a', 'value1'
            0 row(s) in 0.0640 seconds

            hbase(main):001:0> put 'test', 'row2', 'cf:b', 'value2'
            0 row(s) in 0.0040 seconds

            hbase(main):001:0> put 'test', 'row3', 'cf:c', 'value3'
            0 row(s) in 0.0050 seconds

    1. scan table for all data at once

            hbase(main):001:0> scan 'test'
            ROW                      COLUMN+CELL                                                        
             row1                    column=cf:a, timestamp=1434334223890, value=value1                 
             row2                    column=cf:b, timestamp=1434334238864, value=value2                 
             row3                    column=cf:c, timestamp=1434334250205, value=value3                 
            3 row(s) in 0.0250 seconds

    1. get a single row of data

            hbase(main):001:0> get 'test', 'row1'
            COLUMN                   CELL                                                               
             cf:a                    timestamp=1434334223890, value=value1                              
            1 row(s) in 0.0190 seconds

    1. disable a table

            hbase(main):001:0> disable 'test'
            0 row(s) in 1.1770 seconds

            hbase(main):011:0> enable 'test'
            0 row(s) in 0.1510 seconds

    1. drop table

            hbase(main):001:0> disable 'test'
            0 row(s) in 1.1620 seconds

            hbase(main):001:0> drop 'test'
            0 row(s) in 0.1490 seconds

    1. exit the hbase shell

            hbase(main):001:0> quit

    1. stop hbase

            $ stop-hbase.sh
            stopping hbase.......

1. pseudo-distributed local install

    1. stop hbase if it is running

            $ stop-hbase.sh

    1. configure habse `hbase-site.xml`

            <property>
                <name>hbase.cluster.distributed</name>
                <value>true</value>
            </property>

            <property>
                <name>hbase.rootdir</name>
                <value>hdfs://localhost:9000/hbase</value>
            </property>

            # hbase.cluster.distributed
            # directs hbase to run in distributed mode
            # with one jvm instance per daemon

            # hbase.rootdir
            # if you create directory
            # hbase will attempt to do a migration

    1. start hbase

            $ start-hbase.sh

    1. check hbase directory in hdfs

            $ hdfs dfs -ls /hbase
            Found 8 items
            drwxr-xr-x   - hqlgree2 supergroup          0 2015-06-15 10:02 /hbase/.tmp
            drwxr-xr-x   - hqlgree2 supergroup          0 2015-06-15 09:51 /hbase/WALs
            drwxr-xr-x   - hqlgree2 supergroup          0 2015-06-15 10:20 /hbase/archive
            drwxr-xr-x   - hqlgree2 supergroup          0 2015-06-04 10:27 /hbase/corrupt
            drwxr-xr-x   - hqlgree2 supergroup          0 2015-05-31 22:41 /hbase/data
            -rw-r--r--   3 hqlgree2 supergroup         42 2015-05-31 22:41 /hbase/hbase.id
            -rw-r--r--   3 hqlgree2 supergroup          7 2015-05-31 22:41 /hbase/hbase.version
            drwxr-xr-x   - hqlgree2 supergroup          0 2015-06-15 10:13 /hbase/oldWALs

    1. create table and populate it with data

    1. start and stop a backup hbase master server (HMaster)

            $ local-master-backup.sh 2 3 5
            # 16012 16022 16032
            # 16013 16023 16033
            # 16015 16025 16035

    1. kill master with port offset 1

            $ cat /tmp/hbase-hqlgree2-1-master.pid | xargs kill -9

    1. start and stop additional RegionServer

            $ local-regionservers.sh start 2 3 4 5

            # the offset of the server to stop
            $ local-regionservers.sh stop 3

1. fully distributed

    * topology

               hostname-a       hostname-b       hostname-c
            +--------------+ +--------------+ +--------------+
            |              | |              | |              |
            | Master       | | backup       | |              |
            | ZooKeeper    | | ZooKeeper    | | ZooKeeper    |
            |              | | RegionServer | | RegionServer |
            |              | |              | |              |
            +--------------+ +--------------+ +--------------+

    1. configure passwordless ssh access

            $ ssh-keygen -t rsa -P ""
            $ scp ~/.ssh/id_rsa.pub user@hostname-b:~/.ssh/
            $ scp ~/.ssh/id_rsa.pub user@hostname-c:~/.ssh/

            # on node a, b, c
            $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    1. prepare `node-a`

            # 1. edit `conf/regionservers`
            hostname-a
            hostname-b
            hostname-c

            # 2. create `conf/backup-masters`
            hostname-b

            # 3. edit `conf/hbase-site.xml`
            # this cfg will direct `hbase`
            # to start and manage a zookeeper instance
            # on each node of the cluster
            <property>
                <name>hbase.zookeeper.quorum</name>
                <value>hostname-a,hostname-b,hostname-c</value>
            </property>
            <property>
                <name>hbase.zookeeper.property.dataDir</name>
                <value>/usr/local/zookeeper</value>
            </property>

    1. prepare `node-b` and `node-c`

            # run a backup master server
            # and a zookeeper instance
            # 1. setup hbase just as standalone
            # in conf/ create a file `backup-masters`
            $ pico backup-masters
            hostname-b

            # 2. copy configuration
            # node-a => node-b
            # node-a => node-c

    1. start and test cluster

            # 1. be sure hbase is not running on any node

            # 2. start the cluster on `node-a`
            $ start-hbase.sh
            # zookeeper starts first
            # followed by the master
            # then the regionservers
            # finally the backup master

            # 3. verify the process are running
            # on `node-a`
            $ jps
            20355 Jps
            20071 HQuorumPeer
            20137 HMaster

            # on `node-b`
            $ jps
            15930 HRegionServer
            16194 Jps
            15838 HQuorumPeer
            16010 HMaster

            # on `node-c`
            $ jps
            13901 Jps
            13639 HQuorumPeer
            13737 HRegionServer

            # 4. brewse webui
            http://hostname-a:16610
            http://hostname-b:16610

1. [demo config](http://freddy.cellcore.org/post/52568231952/hadoop-hbase-on-osx-10-8-mountain-lion)

    1. `hbase-env.sh`

            $ pico hbase-env.sh
            # export HBASE_OPTS="-XX:+UseConcMarkSweepGC"
            export HBASE_OPTS="-XX:+UseConcMarkSweepGC -Djava.net.preferIPv4Stack=true -Djava.security.krb5.realm=OX.AC.UK -Djava.security.krb5.kdc=kdc0.ox.ac.uk:kdc1.ox.ac.uk"

    1. `hbase-site.xml`

            $ pico hbase-site.xml
            <?xml version="1.0"?>
            <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
            <configuration>
                <property>
                    <name>hbase.rootdir</name>
                    <value>hdfs://localhost:9000/hbase</value>
                    <description>the directory shared by region servers</description>
                </property>
            </configuration>

    1. `~/.bash_profile`

            $ pico .bash_profile
            # hbase home
            export HBASE_HOME=/usr/local/hbase/libexec

    1. start hbase

            $ start-hbase.sh
            starting master, logging to /usr/local/hbase/libexec/logs/hbase-hqlgree2-master-hqlgree2.local.out
            SLF4J: Class path contains multiple SLF4J bindings.
            SLF4J: Found binding in [jar:file:/usr/local/Cellar/hbase/1.0.1/libexec/lib/slf4j-log4j12-1.7.7.jar!/org/slf4j/impl/StaticLoggerBinder.class]
            SLF4J: Found binding in [jar:file:/usr/local/Cellar/hadoop/2.7.0/libexec/share/hadoop/common/lib/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
            SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
            SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]