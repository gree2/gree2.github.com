---
layout: post
title: "mysql too many connections"
description: ""
category: [database]
tags: [mysql, processlist, kill]
---
{% include JB/setup %}


### too many connections

1. steps

    1. list connections and kill it

            mysql> show  processlist;
            ...
            mysql> kill 111111;

    1. config

            $ nano /etc/my.cnf
            max_connections=1000
            wait_timeout=5
