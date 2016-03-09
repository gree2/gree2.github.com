---
layout: post
title: "apache sqoop on mac osx yosemite"
description: ""
category: [bigdata]
tags: [sqoop]
---
{% include JB/setup %}


### prerequisites

1. [hadoop must be installed]({% post_url 2015-04-17-apache-hadoop-on-mac-osx-yosemite %})

1. [brew must be installed]({% post_url 2015-03-08-setting-up-my-mac %})

### [steps](http://springandgrailsmusings.blogspot.com/2012/12/install-sqoop-and-hbase-on-macbook-pro.html)

1. install sqoop via brew

            $ brew install sqoop

1. homebrew take care of installing all related dependencies

            hbase
            zookeeper

### [sqoop 5 minutes demo](https://sqoop.apache.org/docs/1.99.6/Sqoop5MinutesDemo.html)

1. starting client

            $ sqoop2-shell

1. configure client to use sqoop server

            sqoop:000> set server --host localhost --port 12000 --webapp sqoop

1. verigy connection

            sqoop:000> show version --all
            client version:
              Sqoop 2.0.0-...

            server version:
              ...

1. creating link object

    * check for the registered connector on sqoop server

            sqoop:000> show connector

    * `generic jdbc connector` persistence id 2

            sqoop:000> create link -c 2
            ...

            new link was successfully created with validation status OK and persistence id 1

    * `hdfs connector`

            sqoop:000> create link -c 1

1. creating job object

    * show link

            sqoop:000> show link --all

    * use two link ids to associate the `from` and `to` for job

            sqoop:000> create job -f 1 -t 2
            Creating job for links with from id 1 to id 2
            ...
            New job was successfully created with validation status OK and persistence id 1

1. start job (a.k.a data transfer)

    * start a sqoop job

            sqoop:000> start job -j 1

    * check running job status

            sqoop:000> status job -j 1

    * start a sqoop job and observe job running status

            sqoop:000> start job -j 1 -s

    * stop job

            sqoop:000> stop job -j 1