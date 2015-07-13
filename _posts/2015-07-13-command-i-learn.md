---
layout: post
title: "command i learn"
description: ""
category: [command]
tags: [sysctl, wget, grep]
---
{% include JB/setup %}


### commands

1. sysctl

    1. [ip_local_port_range]((http://zookeeper-user.578899.n2.nabble.com/Zookeeper-listening-to-ports-other-than-clientPort-and-server-x-ports-td7580137.html#a7580138))

            $ sysctl net.ipv4.ip_local_port_range
            net.ipv4.ip_local_port_range = 32768    61000

1. wget

    1. [spider a website and return urls only](http://stackoverflow.com/questions/2804467/spider-a-website-and-return-urls-only)

            wget --spider --force-html -r -l2 http://mirrors.cnnic.cn/apache/ 2>&1 \
            | grep '^--' | awk '{ print $3 }' \
            | grep -v '\.\(css\|js\|png\|gif\|jpg\)$' \
            > urls.m3u