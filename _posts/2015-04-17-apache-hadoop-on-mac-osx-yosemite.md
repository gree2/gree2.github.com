---
layout: post
title: "apache hadoop on mac osx yosemite"
description: ""
category: [apache]
tags: [hadoop, max, osx, yosemite]
---
{% include JB/setup %}


### install hadoop

            $ brew install hadoop
            ==> Downloading http://www.apache.org/dyn/closer.cgi?path=hadoop/common/hadoop-2.6.0/h
            ==> Best Mirror http://mirrors.hust.edu.cn/apache/hadoop/common/hadoop-2.6.0/hadoop-2.
            ######################################################################## 100.0%
            ==> Caveats
            In Hadoop's config file:
              /usr/local/Cellar/hadoop/2.6.0/libexec/etc/hadoop/hadoop-env.sh,
              /usr/local/Cellar/hadoop/2.6.0/libexec/etc/hadoop/mapred-env.sh and
              /usr/local/Cellar/hadoop/2.6.0/libexec/etc/hadoop/yarn-env.sh
            $JAVA_HOME has been set to be the output of:
              /usr/libexec/java_home
            ==> Summary
            🍺  /usr/local/Cellar/hadoop/2.6.0: 6140 files, 307M, built in 8.9 minutes


* hadoop will be installed in the directory `/usr/local/Cellar/hadoop`

### configuring hadoop

1. edit hadoop-env.sh

    * locate at `/usr/local/Cellar/hadoop/2.6.0/libexec/etc/hadoop/hadoop-env.sh`

            where 2.6.0 is the hadoop version

    * find the line with

            export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true"

    * and change it to

            export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true -Djava.security.krb5.realm= -Djava.security.krb5.kdc="

1. edit `core-site.xml`

    * lacate at `/usr/local/Cellar/hadoop/2.6.0/libexec/etc/hadoop/core-site.xml`

            <configuration>
                <property>
                    <name>hadoop.tmp.dir</name>  
                    <value>/usr/local/Cellar/hadoop/hdfs/tmp</value>
                    <description>A base for other temporary directories.</description>
                </property>
                <property>
                    <name>fs.default.name</name>
                    <value>hdfs://localhost:9000</value> 
                </property>
            </configuration> 

1. edit `mapred-site.xml`

    * locate at `/usr/local/Cellar/hadoop/2.6.0/libexec/etc/hadoop/mapred-site.xml`

    * by default it will be blank

            <configuration>
                <property>
                    <name>mapred.job.tracker</name>
                    <value>localhost:9010</value>
                </property>
             </configuration>

1. edit `hdfs-site.xml`

    * locate at `/usr/local/Cellar/hadoop/2.6.0/libexec/etc/hadoop/hdfs-site.xml`

            <configuration>
                <property>
                    <name>dfs.replication</name>
                    <value>1</value>
                </property>
             </configuration> 

    * add `hstart` and `hstop` alias

            alias hstart="/usr/local/Cellar/hadoop/2.6.0/sbin/start-dfs.sh;/usr/local/Cellar/hadoop/2.6.0/sbin/start-yarn.sh"
            alias hstop="/usr/local/Cellar/hadoop/2.6.0/sbin/stop-yarn.sh;/usr/local/Cellar/hadoop/2.6.0/sbin/stop-dfs.sh"

    * and execute

            $ source ~/.bash_profile

    * `before we can run hadoop we first need to format the hdfs`

            $ hdfs namenode -format

### ssh localhost

1. check `ssh keys`

    * nothing needs to be done here if you have already generated ssh keys

    * to verify just check for the existance of `~/.ssh/id_rsa` and the `~/.ssh/id_rsa.pub` files

    * if not the keys can be generated using

            $ ssh-keygen -t rsa

1. enable `remote login`

    * check `remote login`

            system preferences -> sharing -> remote login

    * authorize ssh keys

            $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    * try to login

            $ ssh localhost
            > last login: Fri Mar ...
            $ exit

### running hadoop

1. now we can run hadoop just by typing

            $ hstart

1. and stopping using

            $ hstop

### download examples

1. examples

    * [hadoop examples 1.2.1 (old)](https://www.dropbox.com/s/2zthoaww95f5i1y/hadoop-examples-1.2.1.jar?dl=1)

    * [hadoop examples 2.6.0 (current)](https://www.dropbox.com/s/cyuah7lc31g0x3h/hadoop-mapreduce-examples-2.6.0.jar?dl=1)

1. test them out using

            $ hadoop jar </path/to/hadoop-examples file> pi 10 100

### good to know

1. hadoop web interface

    * [Resource Manager http://localhost:50070/](http://localhost:50070/)

    * [JobTracker http://localhost:8088/](http://localhost:8088/)

    * [Specific Node Info http://localhost:8042/](http://localhost:8042/)

### errors

1. faild to start namenode

            $ hdfs namenode

    * and the problem is...

            $ hadoop namenode -format

1. no such file or directory

            $ hstart
            $ hdfs dfs -ls /

    * then we need to create the default directory structure hadoop expects

            $ whoami
            > spaceship
            $ hdfs dfs -mkdir -p /user/spaceship
            > ...
            $ hdfs dfs -ls
            > ...
            $ hdfs dfs -put book.txt
            > ...
            $ hdfs dfs -ls
            > ...
            > found 1 items 