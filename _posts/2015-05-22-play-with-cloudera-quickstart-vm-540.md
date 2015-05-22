---
layout: post
title: "play with cloudera quickstart vm 5.4.0"
description: ""
category: [cloudera]
tags: [hue, hadoop, hbase, impala, spark, solr, oozie]
---
{% include JB/setup %}


### get started

1. tutorial exercise 1 ingest structured data

            $ sqoop import-all-tables -m 1 \
            --connect jdbc:mysql://quickstart:3306/retail_db \
            --username=retail_dba \
            --password=cloudera \
            --compression-codec=snappy \
            --as-avrodatafile \
            --warehouse-dir=/user/hive/warehouse

    * this command take a while to complete

    * export data from our `mysql` database

    * put those export files in `avro` format in `hdfs`

    * alse creating `avro schema`

    * so that we can easily load our `hive` tables for use in `impala` later

            $ hadoop fs -ls /user/hive/warehouse

            $ hadoop fs -ls /user/hive/warehouse/categories

    * `sqoop` also created schema files

            $ ls -l *.avsc

    * `schema` and the data are stored in separate file

    * the schema is only applied when the data si queried, a technique called `schema-on-read`

    * `hive will need schema files

            $ sudo -u hdfs hadoop fs -mkdir /user/examples
            $ sudo -u hdfs hadoop fs -chmod +rw /user/examples
            $ hadoop fs -put ~/*.avsc /user/examples

1. tutorial exercise 2 query structured data

    * use `hue`'s impala app to create the metadata for our tables in hue

    * web-based interface on port 8888 of `master node`

            http://10.0.2.15:8888
            username cloudera
            password cloudera

    * click `query editors` and open `impala query editor`

            create external table categories stored as avro
            location 'hdfs:///user/hive/warehouse/categories'
            tblproperties ('avro.schema.url'='hdfs://quickstart/user/examples/sqoop_import_categories.avsc');

            customers...

            departments...

            orders...

            order_items...

            products...

    * verify all of the tables

            show tables;

    * calculating total revenue per product and showing top 10

            select c.category_name, count(order_item_quantity) as count
            from order_items oi
            inner join products p on oi.order_item_product_id = p.product_id
            inner join categories c on c.category_id = p.product_category_id
            group by c.category_name
            order by count desc
            limit 10;

            select p.product_id, p.product_name, r.revenue
            from products p inner join
            (select oi.order_item_product_id, sum(cast(oi.order_item_subtotal as float)) as revenue
            from order_items oi inner join orders o
            on oi.order_item_order_id = o.order_id
            where o.order_status <> 'CANCELED' and
            o.order_status <> 'SUSPECTED_FRAUD'
            group by order_item_product_id) r
            on p.product_id = r.order_item_product_id
            order by r.revenue desc
            limit 10;

1. tutorial exercise 3 correlate structured data with unstructured data

    * buld upload data

            $ sudo -u hdfs hadoop fs -mkdir /user/hive/warehouse/original_access_logs
            $ sudo -u hdfs hadoop fs -put /opt/examples/log_files/access.log.2 /user/hive/warehouse/original_access_logs

    * verigy data in hdfs

            $ hadoop fs -ls /user/hive/warehouse/original_access_logs

    * query hive using command-line jdbc client for hive called beeline

            $ beeline -u jdbc:hive2://quickstart:10000/default -n admin -d org.apache.hive.jdbc.HiveDriver

    * run query on impala

    * when create new table while impala is running you must tell it to refresh metadata

            # impala query editor
            invalidate metadata;

    * query editor

            select count(*), url from tokenized_access_logs
            where url like '%\/product\/%'
            group by rul order by count(*) desc;