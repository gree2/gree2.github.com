---
layout: post
title: "apache hbase shell"
description: ""
category: [apache]
tags: [habse, shell]
---
{% include JB/setup %}


### usage

1. to invoke non-interactive mode pass `-n` or `--non-interactive` option to hbase shell

1. hbase shell in os scripts

            $ echo "describe 'test'" | hbase shell -n

            $ echo "describe 'test'" | hbase shell -n > /dev/null 2>&1

1. read hbase shell commands from file

            $ pico hbase.cmd
            create 'test', 'cf'
            list 'test'
            put 'test', 'row1', 'cf:a', 'value1'
            put 'test', 'row2', 'cf:b', 'value2'
            put 'test', 'row3', 'cf:c', 'value3'
            put 'test', 'row4', 'cf:d', 'value4'
            scan 'test'
            get 'test', 'row1'
            disable 'test'
            enable 'test'
￼
            $ habse shell ./hbase.cmd

1. passing vm options to shell

            $ HBASE_SHELL_OPTS="-verbose:gc \
            -XX:+PrintGCApplicationStoppedTime \
            -XX:+PrintGCDateStamps \
            -XX:+PrintGCDetails \
            -Xloggc:$HBASE_HOME/log/gc-hbase.log" hbase shell

1. shell tricks

    1. assign a table to a variable by using `get_table` method

            hbase(main):001 > create 't', 'f'
            hbase(main):001 > tab = get_table 't'
            hbase(main):001 > tab.put r1, f, v
            hbase(main):001 > tab.scan

            hbase(main):001 > tables = list( t.* )
            hbase(main):001 > tables.map { |t| disable t; drop t}

1. debug

    1. shell debug switch

            hbase> debug

    1. debug log level

            $ hbase shell -d

1. commands

    1. count

            hbase> count 'test', CACHE => 1000