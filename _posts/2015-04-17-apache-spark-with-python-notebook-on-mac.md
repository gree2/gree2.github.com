---
layout: post
title: "apache spark with python notebook on mac"
description: ""
category: [apache]
tags: [spark, python, notebook, mac]
---
{% include JB/setup %}


### first thing first

* this tutorial if from [installing and running spark with python notebook on mac](http://amodernstory.com/2015/03/05/installing-and-running-spark-with-python-notebook-on-mac/)

* to use spark we need to configure the hadoop eco system of yarn and hdfs

* this can be done following [installing hadoop on yosemite](http://amodernstory.com/2014/09/23/installing-hadoop-on-mac-osx-yosemite/)

### install homebrew

* found here [http://brew.sh/](http://brew.sh/) or `paste` this `inside the terminal`

            ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

* read my [post setting up my mac]({% post_url 2015-03-08-setting-up-my-mac %}) for more info

### to install spark

            $ brew install apache-spark
            ==> Downloading https://d3kbcqa49mib13.cloudfront.net/spark-1.3.0-bin-hadoop2.4.tgz
            ######################################################################## 100.0%
            🍺  /usr/local/Cellar/apache-spark/1.3.0: 327 files, 270M, built in 27.9 minutes

* will install spark to directory `/usr/local/Cellar/apache-spark/1.3.0`

### create python hdfs directory and dataset

1. the dir we will be suing for input and output

            $ hdfs dfs -mkdir /python

1. download a book for word count

            $ wget http://www.gutenberg.org/files/30760/30760-0.txt
            $ mv 30760-0.txt book.txt
            $ hdfs dfs -put book.txt /python/
            $ hdfs dfs -ls /python/

### install anaconda python

* we'll install apaconda for python

    * check my [post anaconda on mac]({% post_url 2015-04-03-anaconda-on-mac %})

### running ipython notebook

* minimizing the verbosity of spark

    * locate on `/usr/local/Cellar/apache-spark/1.3.0/libexec/conf`

            $ cp log4j.properties.template log4j.properties

    * edit `log4j.properties` replace `INFO` with `WARN`

* in the ternimal

            $ IPYTHON_OPTS="notebook --pylab inline" pyspark

    * which starts python, creates the spark hdfs connection, and automatically open up a new browser window with the python notebook

    * in the top right corner click on new notebook

            words = sc.textFile("hdfs://localhost:9000/Python/book.txt")
            words.filter(lambda w: w.startswith(" ")).take(5)
            counts = words.flatMap(lambda line: line.split(" ")) \
                .map(lambda word: (word, 1)) \
                .reduceByKey(lambda a, b: a + b)

            counts.saveAsTextFile("hdfs://localhost:9000/Python/spark_output1")
            counts.collect()

### configure pyspark with ipython notebook

1. create an ipython notebook `profile` for `spark`

            $ ipython profile create spark

            [ProfileCreate] Generating default config file: u'/Users/hqlgree2/.ipython/profile_spark/ipython_config.py'
            [ProfileCreate] Generating default config file: u'/Users/hqlgree2/.ipython/profile_spark/ipython_kernel_config.py'
            [ProfileCreate] Generating default config file: u'/Users/hqlgree2/.ipython/profile_spark/ipython_console_config.py'
            [ProfileCreate] Generating default config file: u'/Users/hqlgree2/.ipython/profile_spark/ipython_qtconsole_config.py'
            [ProfileCreate] Generating default config file: u'/Users/hqlgree2/.ipython/profile_spark/ipython_notebook_config.py'
            [ProfileCreate] Generating default config file: u'/Users/hqlgree2/.ipython/profile_spark/ipython_nbconvert_config.py'

1. create a file in `~/.ipython/profile_spark/startup/00-pyspark-setup.py`

            $ touch 00-pyspark-setup.py

            $ pico 00-pyspark-setup.py
            import os
            import sys

            # Configure the environment
            if 'SPARK_HOME' not in os.environ:
                os.environ['SPARK_HOME'] = '/usr/local/Cellar/apache-spark/1.3.0'

            # Create a variable for our root path
            SPARK_HOME = os.environ['SPARK_HOME']

            # Add the PySpark/py4j to the Python Path
            sys.path.insert(0, os.path.join(SPARK_HOME, "python", "build"))
            sys.path.insert(0, os.path.join(SPARK_HOME, "python"))

1. start up ipython notebook with the profile

            $ ipython notebook --profile spark

1. in your notebook you should see the variables we just created

            print SPARK_HOME

1. at the top of your notebook make sure you add the `spark context`

            from pyspark import SparkContext
            sc = SparkContext( 'local', 'pyspark')

1. test the spark context by doing a simple computation using ipython

            def isprime(n):
                """
                check if integer n is a prime
                """
                # make sure n is a positive integer
                n = abs(int(n))
                # 0 and 1 are not primes
                if n < 2:
                    return False
                # 2 is the only even prime number
                if n == 2:
                    return True
                # all other even numbers are not primes
                if not n & 1:
                    return False
                # range starts with 3 and only needs to go up the square root of n
                # for all odd numbers
                for x in range(3, int(n**0.5)+1, 2):
                    if n % x == 0:
                        return False
                return True

            # Create an RDD of numbers from 0 to 1,000,000
            nums = sc.parallelize(xrange(1000000))

            # Compute the number of primes in the RDD
            print nums.filter(isprime).count()

    * if you get a number without errors, then your context is working correctly