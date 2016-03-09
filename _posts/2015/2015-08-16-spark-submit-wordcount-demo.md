---
layout: post
title: "spark submit wordcount demo"
description: ""
category: [bigdata]
tags: [spark-submit]
---
{% include JB/setup %}


### demo

1. problem

            $ spark-submit --master yarn \
            > --class org.apache.spark.examples.JavaWordCount \
            > --executor-memory 50M --driver-memory 50M \
            > /usr/local/Cellar/apache-spark/1.4.1/libexec/lib/spark-examples-1.4.1-hadoop2.6.0.jar \
            > /user/hive/warehouse/u_data/u.data /out
            Exception in thread "main" java.lang.Exception: When running with master 'yarn' either HADOOP_CONF_DIR or YARN_CONF_DIR must be set in the environment.
                  at org.apache.spark.deploy.SparkSubmitArguments.validateSubmitArguments(SparkSubmitArguments.scala:239)
                  at org.apache.spark.deploy.SparkSubmitArguments.validateArguments(SparkSubmitArguments.scala:216)
                  at org.apache.spark.deploy.SparkSubmitArguments.<init>(SparkSubmitArguments.scala:103)
                  at org.apache.spark.deploy.SparkSubmit$.main(SparkSubmit.scala:107)
                  at org.apache.spark.deploy.SparkSubmit.main(SparkSubmit.scala)
            Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
            15/08/16 13:10:16 INFO Utils: Shutdown hook called

1. set `HADOOP_CONF_DIR` and `HADOOP_YARN_HOME` first

            $ cd
            $ pico .bash_profile
            export HADOOP_PREFIX=/usr/local/hadoop/libexec
            export HADOOP_COMMON_HOME=$HADOOP_PREFIX
            export HADOOP_CONF_DIR=$HADOOP_PREFIX/etc/hadoop
            export HADOOP_HDFS_HOME=$HADOOP_PREFIX
            export HADOOP_MAPRED_HOME=$HADOOP_PREFIX
            export HADOOP_YARN_HOME=$HADOOP_PREFIX

1. rerun demo

            $ spark-submit --master yarn \
            --class org.apache.spark.examples.JavaWordCount \
            --executor-memory 50M --driver-memory 50M \
            /usr/local/apache-spark/libexec/lib/spark-examples-1.4.1-hadoop2.6.0.jar \
            /user/hqlgree2/hive-site.xml out
