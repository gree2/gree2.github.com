---
layout: post
title: "apache hbase getting started"
description: ""
category: [apache]
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