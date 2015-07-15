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

1. locale

    1. [set locale zh_CN.UTF-8](http://askubuntu.com/questions/162391/how-do-i-fix-my-locale-issue/229512#229512)

            $ sudo locale-gen zh_CN.UTF-8
            $ sudo dpkg-reconfigure locales

    1. [ssh server](https://wiki.debian.org/Locale)

            $ sudo pico /etc/ssh/sshd_config
            # conment out this line
            # AcceptEnv LANG LC_*

            # restart ssh
            $ /etc/init.d/ssh restart

    1. [ssh client](https://wiki.debian.org/Locale)

            $ sudo pico /etc/ssh/ssh_config
            # conment out this line
            # SendEnv LANG LC_*

1. tr

    1. [squeeze spaces](http://stackoverflow.com/questions/7142735/linux-cut-help-how-to-specify-more-spaces-for-the-delimiter)

            $ fab sete hd1 | grep part-m-00000 | tr -s ' ' | cut -f 10 -d ' '
            # get hdfs dfs -ls -R output's file path

