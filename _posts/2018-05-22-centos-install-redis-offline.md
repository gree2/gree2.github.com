---
layout: post
title: "centos install redis offline"
description: ""
category: [linux, setup]
tags: [centos, yum, offline, ]
---
{% include JB/setup %}


### steps

1. install redis

    1. add epel repo

            $ yum install epel-release

    1. download redis rpm and deps

            $ yumdownloader --resolve redis
            ...
            (1/2): jemalloc-3.6.0-1.el7.x86_64.rpm                     | 105 kB   00:07
            (2/2): redis-3.2.10-2.el7.x86_64.rpm                       | 545 kB   00:17

    1. install offline

            $ yum -y localinstall jemalloc-3.6.0-1.el7.x86_64.rpm redis-3.2.10-2.el7.x86_64.rpm

    1. start redis

            $ systemctl start redis
            $ systemctl enable redis

1. verify installation

    1. use redis-cli

            $ redis-cli ping
            PONG
            $ redis-cli help
            redis-cli 3.2.11

### references

1. [install and configure redis on centos 7](https://www.linode.com/docs/databases/redis/install-and-configure-redis-on-centos-7/)
