---
layout: post
title: "pdsh parallel distributed shell"
description: ""
category: [linux]
tags: [pdsh, shell]
---
{% include JB/setup %}


### usages

1. steps

    1. install

            $ brew install pdsh

    1. usage

            $ pdsh -w "node[1-4]" -l root date
            $ pdsh -w "node[1-4]" -l root uptime

    1. sync ntp

            $ pdsh -w "node[1-4]" -l root service ntpd stop
            $ pdsh -w "node[1-4]" -l root ntpdate 192.168.100.101
            $ pdsh -w "node[1-4]" -l root service ntpd start
