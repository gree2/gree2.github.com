---
layout: post
title: "docker gethue hue up and running"
description: ""
category: [docker, bigdata]
tags: [gethue, hue, cloudera,]
---
{% include JB/setup %}


### docker hue

1. steps

    1. pull and run

            $ docker pull gethue/hue:latest
            $ docker run -d -p 8888:8888 --name ghue gethue/hue:latest

    1. get inet addr

            $ docker exec -it ghue /sbin/ifconfig eth0

1. usage

    1. visit [http://192.168.99.100:8888](http://192.168.99.100:8888)

            # welcome to hue
            create your hue account

            since this is your first time logging in,
            pick any username and password.
            be sure to remember these,
            as they will become your hue superuser credentials

            username:hue
            password:hue

    1. quick start wizard - hue 3.10.0 - the hadoop ui

            step 1: check configuration

            located in /hue/desktop/conf

            hadoop.hdfs_clusters.default.webhdfs_url    Current value: http://localhost:50070/webhdfs/v1 Failed to access filesystem root
            Resource Manager    Failed to contact an active Resource Manager: HTTPConnectionPool(host='localhost', port=8088): Max retries exceeded with url: /ws/v1/cluster/apps?user=hue (Caused by NewConnectionError(': Failed to establish a new connection: [Errno 111] Connection refused',))

            desktop.secret_key  Current value: Secret key should be configured as a random string. All sessions will be lost on restart

            SQLITE_NOT_FOR_PRODUCTION_USE   SQLite is only recommended for small development environments with a few users.

            Hive    Failed to access Hive warehouse: /user/hive/warehouse

            HBase Browser   The application won't work without a running HBase Thrift Server v1.

            Impala  No available Impalad to send queries to.

            Oozie Editor/Dashboard  The app won't work without a running Oozie server

            Pig Editor  The app won't work without a running Oozie server

            Spark   The app won't work without a running Livy Spark Server

            step 2: examples

            step 3: users

            step 4: go!

### 1. query editors

1. setup

    1. default librdbms settings in hue.ini

    