---
layout: post
title: "apache spark sql and dataframe guide"
description: ""
category: [apache]
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