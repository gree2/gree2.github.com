---
layout: post
title: "apache hbase shell"
description: ""
category: [bigdata]
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

### fixed

1. can't create table

    1. when create table

            hbase(main):002:0> create 'test', 'cf'

            ERROR: Table already exists: test!

    1. try to disable table

            hbase(main):003:0> disable 'test'

            ERROR: Table test does not exist.

    1. fixed

            # 1. start zookeeper zkCli
            $ bin/zkCli
            Connecting to localhost:2181
            Welcome to ZooKeeper!
            JLine support is enabled

            # 2. list /hbase/table
            [zk: localhost:2181(CONNECTED) 1] ls /hbase/table
            [hbase:meta, hbase:namespace, test]

            # 3. rm this znode
            [zk: localhost:2181(CONNECTED) 2] rmr /hbase/table
            [zk: localhost:2181(CONNECTED) 3] ls /hbase/table 
            Node does not exist: /hbase/table

            # 4. restart hbase then list /hbase/table
            [zk: localhost:2181(CONNECTED) 4] ls /hbase/table
            []
