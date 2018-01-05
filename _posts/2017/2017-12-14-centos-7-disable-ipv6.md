---
layout: post
title: "centos 7 disable ipv6"
description: ""
category: [linux]
tags: [ipv6, disable_ipv6, sysctl]
---
{% include JB/setup %}


### steps

1. method 1

    1. disable all nic

            $ nano /etc/sysctl.conf
            ...
            net.ipv6.conf.all.disable_ipv6=1
            net.ipv6.conf.default.disable_ipv6=1
            ...

    1. disable special nic

            $ nano /etc/sysctl.conf
            ...
            net.ipv6.conf.eno1.disable_ipv6=1

    1. to make the setting affective execute, no reboot required

            $ sysctl -p

1. method 2

    1. use echo

            $ echo 1 > /proc/sys/net/ipv6/conf/all/disable_ipv6
            $ echo 1 > /proc/sys/net/ipv6/conf/default/disable_ipv6

    1. use sysctl

            $ sysctl -w net.ipv6.conf.all.disable_ipv6=1
            $ sysctl -w net.ipv6.conf.default.disable_ipv6=1

### reference

1. [https://www.unixmen.com/disable-ipv6-centos-7/](https://www.unixmen.com/disable-ipv6-centos-7/)
