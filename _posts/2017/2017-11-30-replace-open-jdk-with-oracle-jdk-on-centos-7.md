---
layout: post
title: "replace open jdk with oracle jdk on centos 7"
description: ""
category: [linux]
tags: [open, oracle, jdk, rpm]
---
{% include JB/setup %}


### steps

1. os version

    $ cat /etc/redhat-release
    CentOS Linux release 7.4.1708 (Core)

1. list installed java packages

    $ rpm -qa | grep java
    python-javapackages-3.4.1-11.el7.noarch
    tzdata-java-2017c-1.el7.noarch
    java-1.7.0-openjdk-1.7.0.151-2.6.11.1.el7_4.x86_64
    javapackages-tools-3.4.1-11.el7.noarch
    java-1.7.0-openjdk-headless-1.7.0.151-2.6.11.1.el7_4.x86_64

1. remove installed java packages

    $ rpm -e --nodeps tzdata-java-2017c-1.el7.noarch
    $ rpm -e --nodeps java-1.7.0-openjdk-1.7.0.151-2.6.11.1.el7_4.x86_64
    $ rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.151-2.6.11.1.el7_4.x86_64

1. cp jdk rpm file to remote server

    $ scp jdk-8u151-linux-x64.rpm root@192.168.1.123:/root

1. install oracle jdk

    $ rpm -ivh jdk-8u151-linux-x64.rpm
    $ java -version
    java version "1.8.0_151"
    Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
    Java HotSpot(TM) 64-Bit Server VM (build 25.151-b12, mixed mode)
