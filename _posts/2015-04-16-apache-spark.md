---
layout: post
title: "apache spark"
description: ""
category: [bigdata]
tags: [bigdata, spark]
---
{% include JB/setup %}


### from spark site

[Apache Spark™](http://spark.apache.org/) is a fast and `general engine` for `large-scale data processing`

1. speed

	* run programs up to 100x faster than hadoop mapreduce in memory, or 10x faster on disk

	* spark has an advanced `DAG` execution engine that supports cyclic data flow and in-memory computing

1. ease of use

	* write apps quickly in java, scala or python

	* spark offers over 80 high-level operators that make it easy to build parallel apps

	* and you can use it interactively from the scala and python shells

1. generality

	* combine sql, streaming, and complex analytics

	* spark powers a stack of high-level tools including [Spark SQL](http://spark.apache.org/sql/), [MLlib](http://spark.apache.org/mllib/) for machine learning, [GraphX](http://spark.apache.org/graphx/), and [Spark Streaming](http://spark.apache.org/streaming/)

	* you can combine these libraries seamlessly in the same app

				+-----------+ +-----------+ +-----------+ +-----------+
				|           | |           | |           | |           |
				|   spark   | | spark     | |  MLlib    | |  GraphX   |
				|   sql     | | streaming | |  machine  | |  graph    |
				|           | |           | |  learning | |           |
				|           | |           | |           | |           |
				+-----------+ +-----------+ +-----------+ +-----------+

				+-----------------------------------------------------+
				|                                                     |
				|                 apache       spark                  |
				|                                                     |
				+-----------------------------------------------------+

1. runs everywhere

	* runs on

		1. hadoop

		1. mesos

		1. standalone

		1. in the cloud

	* it can access diverse data sources including

		1. hdfs

		1. cassandra

		1. hbase

		1. s3

	* you can run spark readily using its

		1. [standalone cluster mode](http://spark.apache.org/docs/latest/spark-standalone.html)

		1. on [ec2](http://spark.apache.org/docs/latest/ec2-scripts.html)

		1. or run it on hadoop yarn or [apache mesos](http://mesos.apache.org/)

		1. it can read from [hdfs](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html), [hbase](http://hbase.apache.org/), [cassandra](http://cassandra.apache.org/), and any hadoop data source

