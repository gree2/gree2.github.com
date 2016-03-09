---
layout: post
title: "apache sqoop getting started"
description: ""
category: [bigdata]
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

### setup

1. run sqoop

            $ sqoop
            Warning: /usr/local/Cellar/sqoop/1.4.5/libexec/bin/../../hcatalog does not exist! HCatalog jobs will fail.
            Please set $HCAT_HOME to the root of your HCatalog installation.
            Warning: /usr/local/Cellar/sqoop/1.4.5/libexec/bin/../../accumulo does not exist! Accumulo imports will fail.
            Please set $ACCUMULO_HOME to the root of your Accumulo installation.
            Warning: /usr/local/Cellar/sqoop/1.4.5/libexec/bin/../../zookeeper does not exist! Accumulo imports will fail.
            Please set $ZOOKEEPER_HOME to the root of your Zookeeper installation.
            +======================================================================+
            |                    Error: JAVA_HOME is not set                       |
            +----------------------------------------------------------------------+
            | Please download the latest Sun JDK from the Sun Java web site        |
            |     > http://www.oracle.com/technetwork/java/javase/downloads        |
            |                                                                      |
            | HBase requires Java 1.7 or later.                                    |
            +======================================================================+
            Error: Could not find or load main class org.apache.sqoop.Sqoop

1. [set JAVA_HOME on mac](http://stackoverflow.com/questions/6588390/where-is-java-home-on-osx-yosemite-10-10-mavericks-10-9-mountain-lion-10)

            $ pico .bash_profile
            export JAVA_HOME=`/usr/libexec/java_home -v 1.8`

1. [set HCAT_HOME]({% post_url 2015-05-25-apache-hive-on-mac-osx-yosemite %})

            $ pico .bash_profile
            export HCAT_HOME=/usr/local/Cellar/hive/1.1.0/libexec/hcatalog

1. [installing the jdbc drivers](http://www.cloudera.com/content/cloudera/en/documentation/cdh4/latest/CDH4-Installation-Guide/cdh4ig_topic_13_7.html) and check my post [2015-05-25-apache-hive-on-mac-osx-yosemite]({% post_url 2015-05-25-apache-hive-on-mac-osx-yosemite %}) and [connectors and drivers](https://blogs.apache.org/sqoop/entry/connectors_and_drivers_in_the) and [documentation](http://sqoop.apache.org/docs/1.4.0-incubating/SqoopUserGuide.html#id1763114)

            $ curl -L 'http://www.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.35.tar.gz/from/http://mysql.he.net/' | tar xz
            $ cd mysql-connector-java-5.1.35
            $ cp mysql-connector-java-5.1.35-bin.jar /usr/local/Cellar/sqoop/1.4.5/libexec/lib