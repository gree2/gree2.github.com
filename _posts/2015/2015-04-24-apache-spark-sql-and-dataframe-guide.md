---
layout: post
title: "apache spark sql and dataframe guide"
description: ""
category: [bigdata]
tags: [spark, sql, dataframe, guide]
---
{% include JB/setup %}


### SQLContext

1. to create a basic `SQLContext` all you need is a `SparkContext`

            from pyspark.sql import SQLContext
            sqlContext = SQLContext(sc)

1. you can also create a `HiveContext`

### create dataframes

1. with a SQLContext, apps can create `DataFrame`s from

    * an [existing RDD](https://spark.apache.org/docs/1.3.0/sql-programming-guide.html#interoperating-with-rdds)

    * from a hive table

    * from [data sources](https://spark.apache.org/docs/1.3.0/sql-programming-guide.html#data-sources)

            from pyspark.sql import SQLContext
            sqlContext = SQLContext(sc)

            df = sqlContext.jsonFile("examples/src/main/resources/people.json")

            # displays the content of the DataFrame to stdout
            df.show()

### dataframe operations

1. dataframe provide a domain-specific language for structured data manipulation in [Scala](https://spark.apache.org/docs/1.3.0/api/scala/index.html#org.apache.spark.sql.DataFrame), [Java](https://spark.apache.org/docs/1.3.0/api/java/index.html?org/apache/spark/sql/DataFrame.html) and [Python](https://spark.apache.org/docs/1.3.0/api/python/pyspark.sql.html#pyspark.sql.DataFrame)

            form pyspark.sql import SQLContext
            sqlContext = SQLContext(sc)

            # create the dataframe
            df = sqlContext.jsonFile("examples/src/main/resources/people.json")

            # show the content of the dataframe
            df.show()
            ## age  name
            ## null Michael
            ## 30   Andy
            ## 19   Justin

            # print the schema in a tree format
            df.printSchema()
            ## root
            ##  |-- age: long (nullable = true)
            ##  |-- name: string (nullable = true)

            # select only the name column
            df.select("name").show()
            ## name
            ## Michael
            ## Andy
            ## Justin

            # select everybody but increment the age by 1
            df.select("name", df.age + 1).show()
            ## name    (age + 1)
            ## Michael null
            ## Andy    31
            ## Justin  20

            # select people older than 21
            df.filter(df.name > 21).show()
            ## age name
            ## 30  Andy

            ## count people by age
            df.groupBy("name").count().show()
            ## age  count
            ## null 1
            ## 19   1
            ## 30   1

### running sql queries programmatically

1. the `sql` function on a `SQLContext` enables apps to run sql queries

            from pyspark.sql import SQLContext
            sqlContext = SQLContext(sc)
            df = sqlContext.sql("select * from table")

### inferring the schema using reflection

* spark sql can convert an rdd of row object to a dataframe

* rows are constructed by passing a list of key/value pairs as kwargs to the `Row` class

* the keys of this list define the column names of the table

* the types are inferred by looking at the first row

            from pyspark.sql import SQLContext, Row
            sqlContext = SQLContext(sc)

            # load a text file and convert each line to a Row
            lines = sc.textFile("examples/src/main/resources/people.txt")
            parts = line.map(lambda l: l.split(","))
            people = parts.map(lambda p: Row(name=p[0], age=int(p[1])))

            # infer the schema and register the dataframe as a table
            schemaPeople = sqlContext.inferSchema(people)
            schemaPeople.registerTempTable("people")

            # sql can be run over dataframes that have been registered as a table
            teenagers = sqlContext.sql("select name from people where age >= 13 and age <= 19")

            # the results of sql queries are rdds and support all the normal rdd operations
            teennames = teenagers.map(lambda p: "Name: " + p.name)
            for teenname in teennames.collect():
                print teenname

### programmatically specifying the schema

1. create an rdd of tuples or lists from the origin rdd

1. create the schema represented by a `StructType` matching the structure of tuples or list in the rdd created in the step 1

1. apply the schema to the rdd via `createDataFrame` method provided by `SQLContext`

            # import SQLContext and data types
            from pyspark.sql import *

            # sc is an existing SparkContext
            sqlContext = SQLContext(sc)

            # load a text file and convert each line to a tuple
            lines = sc.textFile("examples/src/main/resources/people.txt")
            parts = lines.map(lambda l: l.split(","))
            people = parts.map(lambda p: (p[0], p[1].strip()))

            # the schema is encoed in a string
            schemaString = "name age"
            fields = [StructField(field_name, StringType(), True) for field_name in schemaString.split()]
            schema = StructType(fields)

            # apply the schema to the rdd
            schemaPeople = sqlContext.createDataFrame(people, schema)

            # register the dataframe as a table
            schemaPeople.registerTempTable("people")

            # sql can be run over dataframe
            results = sqlContext.sql("select name from people")

            # the results of sql queries are rdds
            names = results.map(lambda p: "Name: " + p.name)
            for name in names.collect():
                print name

### data sources

1. generic load/save functions

            df = sqlContext.load("people.parquet")
            df.select("name", "age").save("namesAndAges.parquet")

1. manually specifying options

            df = sqlContext.load("people.json", "json")
            df.select("name", "age").save("nameAndAges.parquet", "parquet")

1. save modes (python)

    * "error"

    * "append"

    * "override"

    * "ignore"

1. parquet files

    * [parquet](http://parquet.io/) is a columnar format that is supported by many other data processing systems

    * spark sql provides support for both `reading` and `writing` parquet files

1. loading data programmatically

            # sqlContext form the provious example is used in this example

            # dataframe from the provious example
            schemaPeople

            # dataframes can be saves as parquet files, maintainint the schema information
            schemaPeople.saveAsParquetFile("people.parquet")

            # read in the parquet file created above
            # parquet files are self-describing so the schema is preserved
            # the result of loading a parquet file is also a dataframe
            parquetFile = sqlContext.parquetFile("people.parquet")

            # parquet files can also be registered as tables and then used in sql statements
            parquetFile.registerTempTable("parquetFile")
            teenagers = sqlContext.sql("select name from parquetFile where age >= 13 and age <= 19")
            teennames = teenagers.map(lambda p: "Name: " + p.name)
            for teenname in teennames.collect():
                print teenname

1. partition discovery

    * table partitioning is a common optimization approach used in systems like hive

    * parquet data source is now able to discover and infer partitioning information automatically

1. shcema merging

    * like ProtocolIBuffer, Avro, and Thrift, parquet also support schema evolution

            # sqlContext from the previous example is used in this example

            # create a simple dataframe stored into a partition directory
            df1 = sqlContext.createDataFrame(sc.parallelize(range(1, 6)).map(lambda i: Row(single=i, double=i*2)))
            df1.save("data/test_table/key=1", "parquet")

            # create another dataframe in a new partition directory
            # adding a new column and dropping an existing column
            df2 = sqlContext.createDataFrame(sc.parallelize(range(6, 11)).map(lambda i: Row(single=i, triple=i *3)))
            df2.save("data/test_table/key=2", "parquet")

            # read the partitioned table
            df3 = sqlContext.parquetFile("data/test_table")
            df3.printSchema()

            # the filnal schema consists of all 3 columns in the parquet files together
            # with the partitioning column appeared in the partition directory path.
            # root
            #  |-- single: int (nullable = true)
            #  |-- double: int (nullable = true)
            #  |-- triple: int (nullable = true)
            #  |-- key: int (nullable = true)

1. configuration

    * configuration of parquet can be done using `setConf` method on `SQLContext`

    * or by running `SET key=value` commands using SQL

            spark.sql.parquet.binaryAsString          false
            spark.sql.parquet.int96AsTimestamp        true
            spark.sql.parquet.cacheMetadata           true
            spark.sql.parquet.compression.codec       gzip
            spark.sql.parquet.filterPushdown          false
            spark.sql.parquet.convertMetastoreParquet true

1. json datasets

    * spark sql can automatically infer the schema of a json dataset and load it as a dataframe

    * `jsonFile` - loads data from a directory of josn files where each line of the files is a json object

    * `jsonRDD` - loads data from an existing rdd where each element of the rdd is a string containing a json object

            # sc is an existing SparkContext
            from pyspark.sql import SQLContext
            sqlContext = SQLContext(sc)

            # a json dataset is pointed to by path
            # the path can be either a single text file
            # or a directory storing text files
            path = "examples/src/main/resources/people.json"
            # created a dataframe from the file(s) pointed to by path
            people = sqlContext.jsonFile(path)

            # the inferred schema can ben visualized using the printSchema() method
            people.printSchema()
            # root
            #  |-- age: integer (nullable = true)
            #  |-- name: string (nullable = ture)

            # register thsi dataframe as a table
            people.registerTempTable("people")

            # sql statements can be run by using the sql method provided by sqlContext
            teenagers = sqlContext.sql("select name from people where age >= 13 and age <= 19")

            # alternatively a dataframe can be created for a json dataset
            # represented by an RDD[string] storing one json object per string
            anotherPeopleRDD = sc.parallelize([
                '{"name":"Yin", "address":{"city":"Columbus","state":"Ohio"}}'])
            anotherPeople = sqlContext.jsonRDD(anotherPeopleRDD)

1. hive table

    * configuration of hive is done by placing your `hive-site.xml` file in conf/

    * working with hive one must construct a `HiveContext`

    * which inherits from SQLContext and adds support for finding tables in the MetaSotre and writing queries using HiveQL

    * in addition on the sql method a HiveContext also provides an hql method which allows queries to be expressed in HiveQL

            from pyspark.sql import HiveContext
            sqlContext = HiveContext(sc)

            sqlContext.sql("create table if not exists src (key INT, value STRING")
            SQLContext.sql("LOAD DATA LOCAL INPATH 'examples/src/main/resources/kv1,txt' INTO TABLE src")

            # queries can be expressed in HiveQL
            results = sqlContext.sql("FROM src SELECT key, value").collect()

1. jdbc to other databases

    * to get started you will need to include the jdbc driver for your particular database on the spark classpath

            SPARK_CLASSPATH=postgresql-9.3-1102-jdbc41.jar bin/spark-shell

    * tables from the remote database can be loaded as a dataframe or spark sql temporary table

            url     the jdbc url to connect to
            dbtable the jdbc table that should be read
            driver  the class name of the jdbc driver needed to connect to this url
            partitionColumn, lowerBound, upperBound, numPartitions

            df = sqlContext.load(source="jdbc", url="jdbc:postgresql:dbserver", dbtable="scheme.tablename")

