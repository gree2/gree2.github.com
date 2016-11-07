---
layout: post
title: "ssh and locale"
description: ""
category: [setup]
tags: [ssh, locale, mac, centos]
---
{% include JB/setup %}


### fixed

1. ssh to centos from mac

    1. setup centos locale

            $ sudo nano /etc/sysconfig/i18n
            LANG="zh_CN.UTF-8"
            SYSFONT="latarcyrheb-sun16"

            $ sudo reboot

    1. setup macos

            $ nano ~/.bash_profile
            export LC_ALL=zh_CN.UTF-8
            export LANG=zh_CN.UTF-8

    1. open new iterm2

            $ ssh root@node
            # done

### reference

1. [in osx lion lang is not set to utf8 how fix](http://stackoverflow.com/questions/7165108/in-osx-lion-lang-is-not-set-to-utf8-how-fix)
