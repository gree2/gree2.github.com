---
layout: post
title: "apache kylin"
description: ""
category: [bigdata, bi]
tags: [kylin, hive, mapreduce, hdfs, hbase, calcite]
---
{% include JB/setup %}


### kylin depends on hadoop eco-system

1. hive

			input source pre-join star schema during cube building

1. mapreduce

			aggregate metrics during cube building

1. hdfs

			store intermediate files during cube building

1. hbase

			store and query data cubes

1. calcite

			sql parsing code generation optimization

### kylin highlight

1. extremely fast olap engine at scale

			kylin is designed to reduce query latency on hadoop for 10+ billions of rows of data to seconds

1. ansi sql interface on hadoop

			kylin offers ansi sql on hadoop and support most ansi sql query functions

1. seamless integration with bi tools

			kylin currently offers integration capability with bi tools like tableau

1. interactive query capability

			user can interact with hadoop data via kylin at sub-second latency

1. molap cube

			user can define a data model and pre-build in kylin with more than 10+ billions of raw data records
