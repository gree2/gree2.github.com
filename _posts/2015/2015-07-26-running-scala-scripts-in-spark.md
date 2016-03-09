---
layout: post
title: "running scala scripts in spark"
description: ""
category: [lang, bigdata]
tags: [scala, spark, spark-shell]
---
{% include JB/setup %}


### demos

1. [wordcount example](https://dataissexy.wordpress.com/2015/02/21/running-scala-scripts-in-spark/)

    1. wc.scala

            $ pico wc.scala
            val text = sc.textFile("hdfs://localhost:9000/user/hqlgree2/linkage/block_1.csv")
            val counts = text.flatMap(line => line.split(",")).map(word => (word, 1)).reduceByKey(_+_)
            counts.collect
            System.exit(0)

    1. firing up shell using the `-i` flag

            $ spark-shell -i wc.scala

1. [spark how to run spark file from spark shell](http://stackoverflow.com/questions/27717379/spark-how-to-run-spark-file-from-spark-shell)

    1. use `:load`

            scala> :load wc.scala

    1. use `-i` flag

            $ spark-shell -i wc.scala

1. [how to package a scala project to a jar with sbt](http://ask.systutorials.com/861/how-to-package-a-scala-project-to-a-jar-with-sbt)

            # 1. a jar of just the project classes
            $ sbt package
            # target/scala-2.x.y/projectname_2.x.y-z.jar

            # 2. continuously building the package
            $ sbt ~package

            # 3. standalone executable jar with dependencies
            $ sbt assembly

            $ java -jar projectname_2.x.y-z.jar [class.with.main.function]
