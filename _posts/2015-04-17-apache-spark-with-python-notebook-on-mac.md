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

* will install spark to directory `/usr/local/Cellar/apache-spark/1.3.0`