---
layout: post
title: "apache spark with python notebook on mac"
description: ""
category: [bigdata]
tags: [spark, python, notebook, mac]
---
{% include JB/setup %}


### first thing first

* this tutorial if from [reference installing and running spark with python notebook on mac](http://amodernstory.com/2015/03/05/installing-and-running-spark-with-python-notebook-on-mac/)

* to use spark we need to configure the hadoop eco system of yarn and hdfs

* this can be done following [reference installing hadoop on yosemite](http://amodernstory.com/2014/09/23/installing-hadoop-on-mac-osx-yosemite/) and my [post apache hadoop on mac osx yosemite]({% post_url 2015-04-17-apache-hadoop-on-mac-osx-yosemite %})

### install homebrew

* found here [reference http://brew.sh/](http://brew.sh/) or `paste` this `inside the terminal`

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

### minimizing the verbosity of spark

1. [reference getting started with spark in python](https://districtdatalabs.silvrback.com/getting-started-with-spark-in-python)

1. locate on `/usr/local/Cellar/apache-spark/1.3.0/libexec/conf`

            $ cp log4j.properties.template log4j.properties

1. edit `log4j.properties` replace `INFO` with `WARN`


### configure pyspark with ipython notebook

1. create an ipython notebook `profile` for `spark` [reference configuring ipython notebook support for pyspark](http://ramhiser.com/2015/02/01/configuring-ipython-notebook-support-for-pyspark/)

            $ ipython profile create spark

            [ProfileCreate] Generating default config file: u'/Users/hqlgree2/.ipython/profile_spark/ipython_config.py'
            [ProfileCreate] Generating default config file: u'/Users/hqlgree2/.ipython/profile_spark/ipython_kernel_config.py'
            [ProfileCreate] Generating default config file: u'/Users/hqlgree2/.ipython/profile_spark/ipython_console_config.py'
            [ProfileCreate] Generating default config file: u'/Users/hqlgree2/.ipython/profile_spark/ipython_qtconsole_config.py'
            [ProfileCreate] Generating default config file: u'/Users/hqlgree2/.ipython/profile_spark/ipython_notebook_config.py'
            [ProfileCreate] Generating default config file: u'/Users/hqlgree2/.ipython/profile_spark/ipython_nbconvert_config.py'

1. create `00-pyspark-setup.py` in `~/.ipython/profile_spark/startup/`

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

1. start up ipython notebook with the profile and [notebook-dir](http://www.dattamsha.com/2014/11/wordcount-spark-ipython-notebook/)

            $ PYSPARK_DRIVER_PYTHON=ipython PYSPARK_DRIVER_PYTHON_OPTS="notebook --profile spark --notebook-dir=/Users/hqlgree2/Documents/hadoop" /usr/local/bin/pyspark

1. in your notebook you should see the variables we just created

            print SPARK_HOME

1. word count example [reference http://nbviewer.ipython.org](http://nbviewer.ipython.org/github/marek5050/Hadoop_Examples/blob/master/SparkieNET.ipynb)

            words = sc.textFile("hdfs://localhost:9000/python/book.txt")
            words.filter(lambda w: w.startswith(" ")).take(5)

            counts = words.flatMap(lambda line: line.split(" ")) \
            .map(lambda word: (word, 1))                         \
            .reduceByKey(lambda a, b: a + b)

            counts.saveAsTextFile("hdfs://localhost:9000/python/spark_output_book")
            counts.collect()

    * you can [download 30760-0.txt](http://www.gutenberg.org/files/30760/30760-0.txt) from notebook cell [reference http://nbviewer.ipython.org](http://nbviewer.ipython.org/github/marek5050/Hadoop_Examples/blob/master/SparkieNET.ipynb)

            import urllib2
            response = urllib2.urlopen('http://www.gutenberg.org/files/30760/30760-0.txt')
            html = response.read()
            file("book_temp.txt","w").write(html)
            words = sc.textFile("book_temp.txt")

            file("book_temp.txt", "w").write(urllib2.urlopen("http://www.gutenberg.org/files/30760/30760-0.txt").read())

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