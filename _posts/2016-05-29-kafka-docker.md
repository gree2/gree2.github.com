---
layout: post
title: "kafka docker"
description: ""
category: [docker]
tags: [kafka, docker, docker-compose]
---
{% include JB/setup %}


### steps

1. install docker-compose

            $ sudo apt install docker-compose

1. download wurstmeister/kafka

            $ docker pull wurstmeister/kafka

1. clone [kafka-docker](https://github.com/wurstmeister/kafka-docker)

            $ git clone https://github.com/wurstmeister/kafka-docker.git

1. usage

    1. cluster

            # start a cluster
            $ docker-compose up -d

            # add more brokers
            $ docker-compose scale kafka=3

            # destory a cluster
            $ docker-compose stop

    1. single broker

            $ docker-compose -f docker-compose-single-broker.yml up

1. kafka shell

    1. interact with kafka cluster via kafka shell

            #                        docker_host_ip zk_host:zk_port
            $ ./start-kafka-shell.sh 192.168.1.30   192.168.1.30:2181

            $ echo $KAFKA_HOME
            /opt/kafka_2.10-0.8.2.0

    1. create a topic

            $ $KAFKA_HOME/bin/kafka-topics.sh --create --topic topic \
              --partitions 4 --zookeeper $ZK --replication-factor 1

    1. describ

            $ $KAFKA_HOME/bin/kafka-topics.sh --describ --topic topic \
              --zookeeper $ZK
            Topic:topic PartitionCount:1    ReplicationFactor:1 Configs:
                Topic: topic    Partition: 0    Leader: 9092    Replicas: 9092  Isr: 9092

    1. start a producter

            $ $KAFKA_HOME/bin/kafka-console-producer.sh --topic=topic \
              --broker-list=`broker-list.sh`
            [2016-05-29 12:30:17,171] WARN Property topic is not valid (kafka.utils.VerifiableProperties)
            this
            is
            from
            producter


    1. start a consumer

            $ $KAFKA_HOME/bin/kafka-console-consumer.sh --topic=topic \
              --zookeeper=$ZK
            this
            is
            from
            producter

1. code

    1. [consumer](https://github.com/gree2/hobby/blob/master/python/p.kafka/kafka.consumer.py)

    1. [producter](https://github.com/gree2/hobby/blob/master/python/p.kafka/kafka.producter.py)

### fixed

1. Error while executing topic command replication factor: 2 larger than available brokers: 1

    1. info

            Error while executing topic command replication factor: 2 larger than available brokers: 1
            kafka.admin.AdminOperationException: replication factor: 2 larger than available brokers: 1
                at kafka.admin.AdminUtils$.assignReplicasToBrokers(AdminUtils.scala:70)
                at kafka.admin.AdminUtils$.createTopic(AdminUtils.scala:171)
                at kafka.admin.TopicCommand$.createTopic(TopicCommand.scala:93)
                at kafka.admin.TopicCommand$.main(TopicCommand.scala:55)
                at kafka.admin.TopicCommand.main(TopicCommand.scala)

    1. fixed

            $ $KAFKA_HOME/bin/kafka-topics.sh --create --topic topic \
              --partitions 4 --zookeeper $ZK --replication-factor 1
