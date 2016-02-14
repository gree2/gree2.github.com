---
layout: post
title: "advanced analytics with spark"
description: ""
category: [bigdata]
tags: [spark, spark-shell, scala]
---
{% include JB/setup %}


### ch02 introduction to data analysis with scala and spark

1. prepare data

    1. pull data from repo

            $ mkdir linkage
            $ cd linkage
            $ curl -o donation.zip http://bit.ly/1Aoywaq
            $ unzip donation.zip
            Archive:  donation.zip
             extracting: block_10.zip            
             extracting: block_1.zip             
             extracting: block_2.zip             
             extracting: block_3.zip             
             extracting: block_4.zip             
             extracting: block_5.zip             
             extracting: block_6.zip             
             extracting: block_7.zip             
             extracting: block_8.zip             
             extracting: block_9.zip             
              inflating: documentation           
              inflating: frequencies.csv  

            # 
            $ ls
            block_1.zip
            block_10.zip
            block_2.zip
            block_3.zip
            block_4.zip
            block_5.zip
            block_6.zip
            block_7.zip
            block_8.zip
            block_9.zip
            documentation
            frequencies.csv

            $ unzip 'block_*.zip'

    1. put to hdfs

            $ hdfs dfs -mkdir linkage
            $ hdfs dfs -put block_*.csv linkage

1. spark shell and SparkContext

    1. launch spark jobs on cluster

            $ spark-shell --master yarn-client

    1. launch a local spark cluster

            # match the numbers of cores available on your machine
            $ spark-shell --master local[*]

            # `n` the number of threads to run
            $ spark-shell --master local[n]
            > sc
            res0: org.apache.spark.SparkContext = org.apache.spark.SparkContext@4a833595

            > sc.[\t]
            accumulable                accumulableCollection      accumulator                addFile                    
            addJar                     addSparkListener           appName                    applicationId              
            asInstanceOf               binaryFiles                binaryRecords              broadcast                  
            cancelAllJobs              cancelJobGroup             clearCallSite              clearFiles                 
            clearJars                  clearJobGroup              defaultMinPartitions       defaultMinSplits           
            defaultParallelism         emptyRDD                   files                      getAllPools                
            getCheckpointDir           getConf                    getExecutorMemoryStatus    getExecutorStorageStatus   
            getLocalProperty           getPersistentRDDs          getPoolForName             getRDDStorageInfo          
            getSchedulingMode          hadoopConfiguration        hadoopFile                 hadoopRDD                  
            initLocalProperties        isInstanceOf               isLocal                    jars                       
            killExecutor               killExecutors              makeRDD                    master                     
            metricsSystem              newAPIHadoopFile           newAPIHadoopRDD            objectFile                 
            parallelize                requestExecutors           runApproximateJob          runJob                     
            sequenceFile               setCallSite                setCheckpointDir           setJobDescription          
            setJobGroup                setLocalProperty           sparkUser                  startTime                  
            statusTracker              stop                       submitJob                  tachyonFolderName          
            textFile                   toString                   union                      version                    
            wholeTextFiles

    1. bringing data from cluster to client

            > val rawblocks = sc.textFile("linkage")
            > rawblocks.first
            > val head = rawblocks.take(10)
            > head.length
            > head.foreach(println)
            > def isHeader(line: String) = line.contains("id_1")
            > head.filter(isHeader).foreach(println)
            > head.filterNot(isHeader).length
            > head.filter(x => !isHeader(x)).length
            > head.filter(!isHeader(_)).length

    1. shipping code from client to cluster

            > val noheader = rawblocks.filter(x => !isHeader(x))
            > noheader.first

    1. structuring data with tuples

            > val line = head(5)
            > val pieces = line.split(',')
            > val id1 = pieces(0).toInt
            > val id2 = pieces(1).toInt
            > matched = pieces(11).toBoolean
            > val rawscores = pieces.slice(2, 11)
            > rawscores.map(s => s.toDouble)
            > def toDouble(s: String) = {
                 | if ("?".equals(s)) Double.NaN else s.toDouble
                 | }
            > val scores = rawscores.map(toDouble)

            > def parse(line: String) = {
                 |   val pieces = line.split(',')
                 |   val id1 = pieces(0).toInt
                 |   val id2 = pieces(1).toInt
                 |   val scores = pieces.slice(2, 11).map(toDouble)
                 |   val matched = pieces(11).toBoolean
                 |   (id1, id2, scores, matched)
                 | }
            > val tup = parse(line)
            > tup._1
            > tup.productElement(0)
            > tup.productArity

            > case class MatchData(id1: Int, id2: Int, scores: Array[Double], matched: Boolean)
            > def parse(line: String) = {
                 |   val pieces = line.split(',')
                 |   val id1 = pieces(0).toInt
                 |   val id2 = pieces(1).toInt
                 |   val scores = pieces.slice(2, 11).map(toDouble)
                 |   val matched = pieces(11).toBoolean
                 |   MatchData(id1, id2, scores, matched)
                 | }
            > val md = parse(line)
            > md.matched
            > md.id1

            > val mds = head.filter(!isHeader(_)).map(parse())
            > val parsed = noheader.map(parse)
            > parsed.cache()

    1. aggregations

            > val grouped = mds.groupBy(md => md.matched)
            > grouped.mapValues(x => x.size).foreach(println)

    1. creating histograms

            > val matchCounts = parsed.map(md => md.matched).countByValue()
            > val matchCountsSeq = matchCounts.toSeq


            > matchCountsSeq.sortBy(_._1).foreach(println)
            > matchCountsSeq.sortBy(_._2).foreach(println)
            > matchCountsSeq.sortBy(_._2).reverse.foreach(println)

