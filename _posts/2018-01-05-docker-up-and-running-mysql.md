---
layout: post
title: "docker up and running mysql"
description: ""
category: [docker]
tags: [docker, mysql]
---
{% include JB/setup %}


### docs

1. version

        $ docker pull tomcat:8.0.3
        $ docker pull tomcat:5.7.20
        $ docker pull tomcat:5.6.38
        $ docker pull tomcat:5.5.58

1. how to

    1. start mysql instance

            $ docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=password -d mysql:5.6.38

    1. link container

            $ docker run --name some-app --link some-mysql:mysql -d application-that-uses-mysql

    1. cli

            $ docker run -it --link some-mysql:mysql --rm mysql sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'

    1. cli to remote mysql server

            $ docker run -it --rm mysql mysql -hsome.mysql.host -usome-mysql-user -p

1. shell access and log

        $ docker exec -it some-mysql bash
        $ docker logs some-mysql

1. config

    1. use config file

            $ docker run --name some-mysql -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

    1. without `cnf` file

            $ docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci

1. env

        MYSQL_ROOT_PASSWORD
        MYSQL_DATABASE
        MYSQL_USER, MYSQL_PASSWORD
        MYSQL_ALLOW_EMPTY_PASSWORD
        MYSQL_RANDOM_ROOT_PASSWORD
        MYSQL_ONETIME_PASSWORD

1. data

    1. store data

            $ docker run --name some-mysql -v /my/own/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

    1. dump data

            $ docker exec some-mysql sh -c 'exec mysqldump --all-databases -uroot -p"$MYSQL_ROOT_PASSWORD"' > /some/path/on/your/host/all-databases.sql

1. demo

        $ docker run --name mysql_10  \
        -p 0.0.0.0:30610:3306         \
        -v $(pwd)/data:/var/lib/mysql \
        -e MYSQL_ROOT_PASSWORD=123456 \
        -d mysql:5.6.38               \
        --character-set-server=utf8   \
        --collation-server=utf8_unicode_ci

### references

1. [https://store.docker.com/images/mysql](https://store.docker.com/images/mysql)
