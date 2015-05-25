---
layout: post
title: "apache hive on mac osx yosemite"
description: ""
category: [apache]
tags: [hive, osx, mysql]
---
{% include JB/setup %}


### prerequisites

1. [hadoop must be installed]({% post_url 2015-04-17-apache-hadoop-on-mac-osx-yosemite %})

1. [brew must be installed]({% post_url 2015-03-08-setting-up-my-mac %})

### steps

1. install hive via brew

            $ brew install hive

1. add `hadoop` and `hive` to path

            $ pico ~/.bash_profile
            export HADOOP_HOME=/usr/local/Cellar/hadoop/hadoop.version.no
            export HIVE_HOME=/usr/local/Cellar/hive/hive.version.no/libexec

            $ source ~/.bash_profile

1. download mysql connector

            $ curl -L 'http://www.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.22.tar.gz/from/http://mysql.he.net/' | tar xz

            # copy the jar to `hive lib folder`
            $ sudo cp mysql-connector-java-5.1.15/mysql-connector-java-5.1.22-bin.jar /usr/local/Cellar/hive/hive.version.no/libexec/lib/

1. create mysql metastore

            $ mysql
            mysql> create database metastore;
            mysql> use metastore;
            mysql> create user 'hiveuser'@'localhost' identified by 'password';
            mysql> grant select, insert, update, delete, alter, create on metastore.* to 'hiveuser'

1. copy template to `hive-site.xml`

            $ cd /usr/local/Cellar/hive/hive.version.no/libexec/conf
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