---
layout: post
title: "hbase test column family"
description: ""
category: [bigdata]
tags: [hbase, column family, column qualifier, create, scan, put, get]
---
{% include JB/setup %}


### steps

1. `test` table

    1. create

            hbase(main):001:0> create 'test', 'cf'
            0 row(s) in 1.4760 seconds

            => Hbase::Table - test

    1. list

            hbase(main):002:0> list 'test'
            TABLE                                                                                                                                                          
            test                                                                                                                                                           
            1 row(s) in 0.0230 seconds

            => ["test"]

1. row key `20160124-00001-01` and column qualifier `0001` to `0005`

            hbase(main):003:0> put 'test', '20160124-00001-01', 'cf:0001', 1
            0 row(s) in 0.7780 seconds

            hbase(main):004:0> put 'test', '20160124-00001-01', 'cf:0002', 2
            0 row(s) in 0.0040 seconds

            hbase(main):005:0> put 'test', '20160124-00001-01', 'cf:0003', 3
            0 row(s) in 0.0130 seconds

            hbase(main):006:0> put 'test', '20160124-00001-01', 'cf:0004', 4
            0 row(s) in 0.0120 seconds

            hbase(main):007:0> put 'test', '20160124-00001-01', 'cf:0005', 5
            0 row(s) in 0.0030 seconds

1. scan test table

            hbase(main):008:0> scan 'test'
            ROW                                      COLUMN+CELL                                                                                                           
             20160124-00001-01                       column=cf:0001, timestamp=1453624917237, value=1                                                                      
             20160124-00001-01                       column=cf:0002, timestamp=1453624924582, value=2                                                                      
             20160124-00001-01                       column=cf:0003, timestamp=1453624932430, value=3                                                                      
             20160124-00001-01                       column=cf:0004, timestamp=1453624939495, value=4                                                                      
             20160124-00001-01                       column=cf:0005, timestamp=1453624946075, value=5                                                                      
            1 row(s) in 0.0270 seconds

1. get row 1 `20160124-00001-01`

            hbase(main):009:0> get 'test', '20160124-00001-01'
            COLUMN                                   CELL                                                                                                                  
             cf:0001                                 timestamp=1453624917237, value=1                                                                                      
             cf:0002                                 timestamp=1453624924582, value=2                                                                                      
             cf:0003                                 timestamp=1453624932430, value=3                                                                                      
             cf:0004                                 timestamp=1453624939495, value=4                                                                                      
             cf:0005                                 timestamp=1453624946075, value=5                                                                                      
            5 row(s) in 0.0270 seconds

1. row key `20160124-00001-02` and column qualifier `0001` to `0005`

            hbase(main):010:0> put 'test', '20160124-00001-02', 'cf:0001', 1
            0 row(s) in 0.0030 seconds

            hbase(main):011:0> put 'test', '20160124-00001-02', 'cf:0002', 2
            0 row(s) in 0.0030 seconds

            hbase(main):012:0> put 'test', '20160124-00001-02', 'cf:0003', 3
            0 row(s) in 0.0040 seconds

            hbase(main):013:0> put 'test', '20160124-00001-02', 'cf:0004', 4
            0 row(s) in 0.0040 seconds

            hbase(main):014:0> put 'test', '20160124-00001-02', 'cf:0005', 5
            0 row(s) in 0.0040 seconds

1. scan test table again

            hbase(main):015:0> scan 'test'
            ROW                                      COLUMN+CELL                                                                                                           
             20160124-00001-01                       column=cf:0001, timestamp=1453624917237, value=1                                                                      
             20160124-00001-01                       column=cf:0002, timestamp=1453624924582, value=2                                                                      
             20160124-00001-01                       column=cf:0003, timestamp=1453624932430, value=3                                                                      
             20160124-00001-01                       column=cf:0004, timestamp=1453624939495, value=4                                                                      
             20160124-00001-01                       column=cf:0005, timestamp=1453624946075, value=5                                                                      
             20160124-00001-02                       column=cf:0001, timestamp=1453625019256, value=1                                                                      
             20160124-00001-02                       column=cf:0002, timestamp=1453625028474, value=2                                                                      
             20160124-00001-02                       column=cf:0003, timestamp=1453625038122, value=3                                                                      
             20160124-00001-02                       column=cf:0004, timestamp=1453625045386, value=4                                                                      
             20160124-00001-02                       column=cf:0005, timestamp=1453625053804, value=5                                                                      
            2 row(s) in 0.0250 seconds

1. get row 2 `20160124-00001-02`

            hbase(main):016:0> get 'test', '20160124-00001-02'
            COLUMN                                   CELL                                                                                                                  
             cf:0001                                 timestamp=1453625019256, value=1                                                                                      
             cf:0002                                 timestamp=1453625028474, value=2                                                                                      
             cf:0003                                 timestamp=1453625038122, value=3                                                                                      
             cf:0004                                 timestamp=1453625045386, value=4                                                                                      
             cf:0005                                 timestamp=1453625053804, value=5                                                                                      
            5 row(s) in 0.0100 seconds
