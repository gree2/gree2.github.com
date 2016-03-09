---
layout: post
title: "apache hive on mac osx yosemite"
description: ""
category: [bigdata]
tags: [hive, osx, mysql]
---
{% include JB/setup %}


### prerequisites

1. [hadoop must be installed]({% post_url 2015-04-17-apache-hadoop-on-mac-osx-yosemite %})

1. [brew must be installed]({% post_url 2015-03-08-setting-up-my-mac %})

### steps

1. install hive via brew

            $ brew install hive

            Error: Cannot install hive because conflicting formulae are installed.

            apache-spark: because both install `beeline` binaries

            Please `brew unlink apache-spark` before continuing.

            Unlinking removes a formula's symlinks from /usr/local. You can
            link the formula again after the install finishes. You can --force this
            install, but the build may fail or cause obscure side-effects in the
            resulting software.

            $ brew install hive
            ==> Downloading http://www.apache.org/dyn/closer.cgi?path=hive/hive-1.0.0/apache
            ==> Best Mirror http://mirrors.hust.edu.cn/apache/hive/hive-1.0.0/apache-hive-1.
            ######################################################################## 100.0%
            ==> Caveats
            Hadoop must be in your path for hive executable to work.
            After installation, set $HIVE_HOME in your profile:
              export HIVE_HOME=/usr/local/Cellar/hive/1.0.0/libexec

            If you want to use HCatalog with Pig, set $HCAT_HOME in your profile:
              export HCAT_HOME=/usr/local/Cellar/hive/1.0.0/libexec/hcatalog

            You may need to set JAVA_HOME:
              export JAVA_HOME="$(/usr/libexec/java_home)"
            ==> Summary
            🍺  /usr/local/Cellar/hive/1.0.0: 671 files,  91M, built in 4.6 minutes

1. add `hadoop` and `hive` to path

            $ pico ~/.bash_profile
            export HADOOP_HOME=/usr/local/Cellar/hadoop/2.7.0
            export HIVE_HOME=/usr/local/Cellar/hive/1.1.0/libexec
            export HCAT_HOME=/usr/local/Cellar/hive/1.1.0/libexec/hcatalog

            $ source ~/.bash_profile

1. download mysql connector

            $ curl -L 'http://www.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.22.tar.gz/from/http://mysql.he.net/' | tar xz

            # copy the jar to `hive lib folder`
            $ sudo cp mysql-connector-java-5.1.15/mysql-connector-java-5.1.22-bin.jar /usr/local/Cellar/hive/1.1.0/libexec/lib/

1. create mysql metastore

    * mysql

            $ mysql
            ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
           
    * start mysql

            $ mysql.server start
            Starting MySQL
            . SUCCESS! 

            $ mysql
            Welcome to the MySQL monitor.  Commands end with ; or \g.
            Your MySQL connection id is 1
            Server version: 5.6.23 Homebrew

            Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

            Oracle is a registered trademark of Oracle Corporation and/or its
            affiliates. Other names may be trademarks of their respective
            owners.

            Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

            mysql> create database metastore;
            ERROR 1044 (42000): Access denied for user ''@'localhost' to database 'metastore'

    * login as root

            $ mysql -u root -p
            mysql> create database metastore;
            mysql> use metastore;
            mysql> create user 'hiveuser'@'localhost' identified by 'password';
            mysql> grant select, insert, update, delete, alter, create, index on metastore.* to 'hiveuser'

    * install mysql use homebrew

            $ brew install mysql
            ==> Downloading https://homebrew.bintray.com/bottles/mysql-5.6.23.yosemite.bottl
            ######################################################################## 100.0%
            ==> Pouring mysql-5.6.23.yosemite.bottle.tar.gz
            ==> Caveats
            A "/etc/my.cnf" from another install may interfere with a Homebrew-built
            server starting up correctly.

            To connect:
                mysql -uroot

            To have launchd start mysql at login:
                ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
            Then to load mysql now:
                launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
            Or, if you don't want/need launchctl, you can just run:
                mysql.server start
            ==> /usr/local/Cellar/mysql/5.6.23/bin/mysql_install_db --verbose --user=hqlgree
            ==> Summary
            🍺  /usr/local/Cellar/mysql/5.6.23: 9687 files, 339M


1. copy template to `hive-site.xml`

            $ cd /usr/local/Cellar/hive/1.1.0/libexec/conf
            $ cp hive-default.xml.template hive-site.xml

1. add/edit `hive-site.xml`

            <property>
                <name>javax.jdo.option.ConnectionURL</name>
                <value>jdbc:mysql://localhost/metastore</value>
            </property>
            <property>
                <name>javax.jdo.option.ConnectionDriverName</name>
                <value>com.mysql.jdbc.Driver</value>
            </property>
            <property>
                <name>javax.jdo.option.ConnectionUserName</name>
                <value>hiveuser</value>
            </property>
            <property>
                <name>javax.jdo.option.ConnectionPassword</name>
                <value>password</value>
            </property>
            <property>
                <name>datanucleus.fixedDatastore</name>
                <value>false</value>
            </property>

    * replace

            ${system:java.io.tmpdir}/${system:user.name} => /tmp/mydir

            ${system:java.io.tmpdir}/${hive.session.id} => /usr/local/Cellar/hive/1.0.0/libexec/iotmp

            <property>
                <name>hive.server2.logging.operation.log.location</name>
                <value>/tmp/mydir/operation_logs</value>
                <description>Top level directory where operation logs are stored if logging functionality is enabled</description>
            </property>
            <property>
                <name>hive.exec.local.scratchdir</name>
                <value>/tmp/mydir</value>
                <description>Local scratch space for Hive jobs</description>
            </property>
            <property>
                <name>hive.querylog.location</name>
                <value>/tmp/mydir</value>
                <description>Location of Hive run time structured log file</description>
            </property>
            <property>
                <name>hive.downloaded.resources.dir</name>
                <value>/usr/local/Cellar/hive/1.0.0/libexec/iotmp_resources</value>
                <description>Temporary local directory for added resources in the remote file system.</description>
            </property>

1. test if hive works

            $ hive
            hive> show tables;
            hive> create table temp_table temp_col string;

1. revoke few permissions on the mysql metastore

            $ mysql
            mysql> revoke alter, create on metastore.* from 'hiveuser'@'localhost';

1. further trobleshooting

    * get a bin log error saying statement format is not support

            $ mysql -uroot
            mysql> set global binlog_format = 'ROW';

    * also try reading the los as follows

            $ hive -hiveconf hive.root.logger=INFO,console

    * also read the raw hive logs located at /tmp/user_name/hive.log

### [reference](https://noobergeek.wordpress.com/2013/11/09/simplest-way-to-install-and-configure-hive-for-mac-osx-lion/)