---
layout: post
title: "apache kafka getting started"
description: ""
category: [bigdata]
tags: [kafka, tree]
---
{% include JB/setup %}


### info

1. `apache kafka` is a publish-subcribe messaging rethought as a distributed commit log

1. use cases

    1. `messaging` comparable to traditional messaging systems such as [ActiveMQ](http://activemq.apache.org/) or [RabbitMQ](https://www.rabbitmq.com/)

    1. website activity tracking

    1. metrics

    1. log aggregation

    1. stream processing

    1. event sourcing

    1. commit log

### install

1. dependency

            $ brew deps kafka
            gradle
            python
            zookeeper

1. install

            $ brew install kafka
            ==> Installing kafka dependency: gradle
            ==> Downloading https://downloads.gradle.org/distributions/gradle-2.4-bin.zip
            ######################################################################## 100.0%
            🍺  /usr/local/Cellar/gradle/2.4: 177 files, 48M, built in 2.9 minutes
            ==> Installing kafka
            ==> Downloading https://homebrew.bintray.com/bottles/kafka-0.8.2.1.yosemite.bottle.1.tar.gz
            ######################################################################## 100.0%
            ==> Pouring kafka-0.8.2.1.yosemite.bottle.1.tar.gz
            ==> Caveats
            To start Kafka, ensure that ZooKeeper is running and then execute:
              kafka-server-start.sh /usr/local/etc/kafka/server.properties

            To have launchd start kafka at login:
                ln -sfv /usr/local/opt/kafka/*.plist ~/Library/LaunchAgents
            Then to load kafka now:
                launchctl load ~/Library/LaunchAgents/homebrew.mxcl.kafka.plist
            ==> Summary
            🍺  /usr/local/Cellar/kafka/0.8.2.1: 3817 files, 38M

### quick start

1. single node

    1. download the code

            $ tar -xzf kafka_*.tgz
            $ cd kafka_*

    1. start  the server

            # single-node zookeeper instance
            $ zookeeper-server-start.sh config/zookeeper.properties

            # start the kafka server
            $ kafka-server-start.sh config/server.properties

    1. create a topic

            # create a topic name `test`
            # with single partition
            # and only one replica
            $ kafka-topics.sh --create --zookeeper \
            localhost:2181 --replication-factor 1 \
            --partition 1 --topic test

            # list topic command
            $ kafka-topics.sh --list --zookeeper localhost:2181

    1. send some messages

            # command line client
            # take input form a file
            # form standard input
            # and send out as messages to kafka cluster
            $ kafka-console-producer.sh --broker-list localhost:9092 \
            --topic test
            this is a message
            this is another message

    1. start a consumer

            # command line consumer
            # dump out message to standard output
            $ kafka-console-consumer.sh --zookeeper localhost:2181 \
            --topic test --from-beginning

1. setting up a multi-broker cluster

    1. make a config file for each brokers
            
            $ cp config/server.properties config/server-1.properties
            $ cp config/server.properties config/server-2.properties

            # edit config files
            $ pico config/server-1.properties
            broker.id=1
            port=9093
            log.dir=/tmp/kafka-logs-1

            $ pico config/server-2.properties
            broker.id=2
            port=9094
            log.dir=/tmp/kafka-logs-2

    1. just start two new nodes
    
            $ kafka-server-start.sh config/server-1.properties &
            $ kafka-server-start.sh config/server-2.properties &

    1. create new topic with replication factor of three

            $ kafka-topics.sh --create --zookeeper localhost:2181 \
            --replication-factor 3 --partition 1 --topic my-replicated-topic

    1. find out which broker is doing what

            $ kafka-topics.sh --describe --zookeeper localhost:2181 \
            --topic my-replicated-topic

            $ kafka-topics.sh --describe --zookeeper localhost:2181 \
            --topic test

    1. publish a few messages to our new topic

            $ kafka-console-producer.sh --broker-list localhost:9092 \
            --topic my-replicated-topic
            my test message 1
            my test message 2

    1. consume these messages

            $ kafka-console-consumer.sh --zookeeper localhost:2181 \
            --from-beginning --topic my-replicated-topic
            my test message 1
            my test message 2

    1. test out fault-tolerance

            $ ps | grep server-1.properties
            $ kill -9 7564

    1. leadership has switched to one of the slaves

            $ kafka-topics.sh --describe --zookeeper localhost:2181 \
            --topic my-replicated-topic

    1. messages are still be available

            $ kafka-console-consumer.sh --zookeeper localhost:2181 \
            --from-beginning --topic my-replicated-topic

### demo

1. [running-a-multi-broker-apache-kafka-cluster-on-a-single-node](http://www.michael-noll.com/blog/2013/03/13/running-a-multi-broker-apache-kafka-cluster-on-a-single-node/)

    1. diagram

                                +---------------+                     
                                | kafka cluster |                     
                                |               |                     
                                |  +----------+ |                     
                          +------->| broker1  |-------+               
                          |     |  +----------+ |     |               
                          |     |  +---+  +---+ |     |               
                          |     |  | P0|  | P2| |     |               
                          |     |  | R1|  | R1| |     |               
                          |     |  +---+  +---+ |     |               
            +----------+  |     |               |     |   +----------+
            |          |  |     |  +----------+ |     |   |          |
            | producer ---+------->| broker2  |-------+---->consumer |
            |          |  |     |  +----------+ |     |   |          |
            +----------+  |     |  +---+  +---+ |     |   +----------+
                          |     |  | P0|  | P1| |     |               
                          |     |  | R2|  | R2| |     |               
                          |     |  +---+  +---+ |     |               
                          |     |               |     |               
                          |     |  +----------+ |     |               
                          +------->| broker3  |-------+               
                                |  +----------+ |                     
                                |  +---+  +---+ |                     
                                |  | P1|  | P2| |                     
                                |  | R3|  | R3| |                     
                                |  +---+  +---+ |                     
                                |               |                     
                                |  +----------+ |                     
                                |  | zookeeper| |                     
                                |  +----------+ |                     
                                +---------------+                     

    1. log.dir `/usr/local/var/lib/`

            $ tree kafka-logs kafka-logs1 kafka-logs2
            kafka-logs
            ├── my-replicated-topic-0
            │   ├── 00000000000000000000.index
            │   └── 00000000000000000000.log
            ├── recovery-point-offset-checkpoint
            ├── replication-offset-checkpoint
            └── test-0
                ├── 00000000000000000000.index
                └── 00000000000000000000.log
            kafka-logs1
            ├── my-replicated-topic-0
            │   ├── 00000000000000000000.index
            │   └── 00000000000000000000.log
            ├── recovery-point-offset-checkpoint
            └── replication-offset-checkpoint
            kafka-logs2
            ├── my-replicated-topic-0
            │   ├── 00000000000000000000.index
            │   └── 00000000000000000000.log
            ├── recovery-point-offset-checkpoint
            └── replication-offset-checkpoint