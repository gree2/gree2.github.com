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

1. use hosts file

    1. hosts file

            $ nano hosts.txt
            node1
            node2
            node3
            node4

    1. usage

            $ pdsh -R ssh -w ^hosts.txt -l root date
