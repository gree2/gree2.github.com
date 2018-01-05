---
layout: post
title: "setup kylin with transwarp tdh 4.6"
description: ""
category: [bigdata, setup]
tags: [kylin, transwarp, tdh, 4.6]
---
{% include JB/setup %}


### steps

1. download [apache-kylin-2.0.0-bin-hbase098.tar.gz](http://kylin.apache.org/cn/download/)

1. setup

    1. steps

            $ scp  apache-kylin-2.0.0-bin-hbase098.tar.gz root@node2:/root
            $ ssh root@node2
            $ tar zxvf apache-kylin-2.0.0-bin-hbase098.tar.gz
            $ rsync -avP apache-kylin-2.0.0 /usr/lib/apache-kylin-2.0.0
            $ cd /usr/lib/apache-kylin-2.0.0
            $ export KYLIN_HOME=`pwd`

    1. check

            $ sudo -u hdfs bin/check-env.sh
            Retrieving hadoop conf dir...
            KYLIN_HOME is set to /usr/lib/apache-kylin-2.0.0-bin

    1. start

            $ sudo -u hdfs bin/kylin.sh start
            Retrieving hadoop conf dir...
            KYLIN_HOME is set to /usr/lib/apache-kylin-2.0.0-bin
            Retrieving hive dependency...
            Couldn't find hive configuration directory. Please set HIVE_CONF to the path which contains hive-site.xml.

            $ export HIVE_HOME=/usr/lib/hive/conf
            $ sudo -u hdfs bin/kylin.sh start
            Retrieving hadoop conf dir...
            KYLIN_HOME is set to /usr/lib/apache-kylin-2.0.0-bin
            Retrieving hive dependency...
            Couldn't locate hcatalog installation, please make sure it is installed and set HCAT_HOME to the path.

### failed

1. tdh's inceptor 4.6 does not support kylin

### reference

1. [http://kylin.apache.org/cn/docs20/install/manual_install_guide.html](http://kylin.apache.org/cn/docs20/install/manual_install_guide.html)

1. [http://kylin.apache.org/cn/docs20/install/index.html](http://kylin.apache.org/cn/docs20/install/index.html)

1. [http://kylin.apache.org/cn/docs20/tutorial/create_cube.html](http://kylin.apache.org/cn/docs20/tutorial/create_cube.html)

1. [http://kylin.apache.org/cn/docs20/tutorial/web.html](http://kylin.apache.org/cn/docs20/tutorial/web.html)
