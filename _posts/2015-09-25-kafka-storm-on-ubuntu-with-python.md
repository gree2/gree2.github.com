---
layout: post
title: "kafka storm on ubuntu with python"
description: ""
category: [bigdata]
tags: [kafka, storm, ubuntu, python, kafaka-python, pyleus, nimbus, supervisor, ui, zookeeper]
---
{% include JB/setup %}


### setups

1. ubuntu

    1. topology

            +---------------+ +---------------+ +---------------+
            |      node1    | |      node2    | |      node3    |
            +---------------+ +---------------+ +---------------+
            | 192.168.1.151 | | 192.168.1.152 | | 192.168.1.153 |
            +---------------+ +---------------+ +---------------+
            |    kafaka     | |    kafaka     | |     kafaka    |
            |    storm      | |    storm      | |     storm     |
            |    zookeeper  | |    zookeeper  | |     zookeeper |
            +---------------+ +---------------+ +---------------+

1. zookeeper

    1. topology

            ===============================================
               ↓       ↑       ↓       ↑       ↓       ↑   
               ↓       ↑       ↓       ↑       ↓       ↑   
            +-------------+ +-------------+ +-------------+
            |    node1    | |    node2    | |    node3    |
            +-------------+ +-------------+ +-------------+
            |  zookeeper  | |  zookeeper  | |  zookeeper  |
            +-------------+ +-------------+ +-------------+

1. kafka

    1. communication between the clients and the servers

            +-------------+ +-------------+ +-------------+
            |  producer1  | |  producer2  | |  producer3  |
            +-------------+ +-------------+ +-------------+
                   ↓               ↓               ↓       
            +---------------------------------------------+
            |                kafka cluster                |
            +---------------------------------------------+
                   ↓               ↓               ↓       
            +-------------+ +-------------+ +-------------+
            |  consumer1  | |  consumer2  | |  consumer3  |
            +-------------+ +-------------+ +-------------+

    1. anatomy of topic

                        +-+-+-+-+-+-+-+-+-+
            partition-0 |0|1|2|3|4|5|6|7|x| ←----- write
                        +-+-+-+-+-+-+-+-+-+
                                           
                        +-+-+-+-+-+-+-+-+-+
            partition-1 |0|1|2|3|4|5|x|     ←----- write
                        +-+-+-+-+-+-+-+-+-+
                                           
                        +-+-+-+-+-+-+-+-+-+
            partition-2 |0|1|2|3|4|5|6|x|   ←----- write
                        +-+-+-+-+-+-+-+-+-+

                    old ------------------→ new

1. storm

    1. componet of storm cluster

                                             |   +------------+
                                             | ↔ | supervisor |
                                             |   +------------+
                                             |                 
                         |   +-----------+   |   +------------+
                         | ↔ | zookeeper | ↔ | ↔ | supervisor |
                         |   +-----------+   |   +------------+
                         |                   |                 
            +--------+   |   +-----------+   |   +------------+
            | nimbus | ↔ | ↔ | zookeeper | ↔ | ↔ | supervisor |
            +--------+   |   +-----------+   |   +------------+
                         |                   |                 
                         |   +-----------+   |   +------------+
                         | ↔ | zookeeper | ↔ | ↔ | supervisor |
                         |   +-----------+   |   +------------+
                                             |                 
                                             |   +------------+
                                             | ↔ | supervisor |
                                             |   +------------+

1. python

    1. setups

    1. kafaka demo

    1. storm demo

### 1. ubuntu

1. config ubuntu

    1. ip 

            $ sudo pico /etc/network/interface
            # This file describes the network interfaces available on your system
            # and how to activate them. For more information, see interfaces(5).

            # The loopback network interface
            auto lo
            iface lo inet loopback

            # The primary network interface
            auto eth0
            iface eth0 inet static
            address 192.168.1.151
            netmask 255.255.255.0
            gateway 192.168.1.253
            dns-nameservers 192.168.1.253

    1. hosts

            $ sudo pico /etc/hosts
            127.0.0.1   localhost
            #127.0.1.1  node

            # The following lines are desirable for IPv6 capable hosts
            ::1     localhost ip6-localhost ip6-loopback
            ff02::1 ip6-allnodes
            ff02::2 ip6-allrouters

            192.168.1.151 node1
            192.168.1.152 node2
            192.168.1.153 node3

    1. hostname

            $ sudo pico /etc/hostname
            node1

1. install packages

    1. ssh

            $ sudo apt-get install openssh-server

    1. [oracle jdk]({% post_url 2015-09-24-install-oracle-jdk-on-ubuntu %})

    1. unzip

            $ sudo apt-get install unzip

    1. python - anaconda

### 2. zookeeper

### 3. kafka

1. [reference](http://www.michael-noll.com/blog/2013/03/13/running-a-multi-broker-apache-kafka-cluster-on-a-single-node/)

1. usage

    1. start the kafka server

            # on node1
            $ env JMX_PORT=10002 bin/kafka-server-start.sh config/server.properties

            # on node2
            $ env JMX_PORT=10003 bin/kafka-server-start.sh config/server.properties

            # on node3
            $ env JMX_PORT=10004 bin/kafka-server-start.sh config/server.properties

    1. create a topic

            # on node1
            $ bin/kafka-topics.sh --zookeeper 192.168.1.151:2181 \
            --create --topic topic1 --partitions 3 --replication-factor 2

    1. list topics

            $ bin/kafka-topics.sh --zookeeper 192.168.1.151:2181 --list

    1. describe topic

            $ bin/kafka-topics.sh --zookeeper 192.168.1.151:2181 --describe --topic topic1

    1. send some messages

            $ bin/kafka-console-producer.sh --broker-list \
            192.168.1.151:9092,192.168.1.152:9093,192.168.1.153:9094 \
            --sync --topic topic1

    1. start a consumer

            $ bin/kafka-console-consumer.sh --zookeeper 192.168.1.151:2181 --topic topic1 --from-beginning

### 4. storm

1. [reference](https://storm.apache.org/documentation/Setting-up-a-Storm-cluster.html)

1. usage

    1. start nimbus

            # on node1
            $ bin/storm nimbus >/dev/null 2>&1 &

    1. start supervisor

            # on node1 node2 node3
            $ bin/storm supervisor >/dev/null 2>&1 &

    1. start ui

            # on node1
            $ bin/storm ui >/dev/null 2>&1 &

    1. visit

            http://node1:8080

### 5. python

1. setup

    1. on node

            # on dev
            $ scp anaconda-2.3.0-linux-x86_64.sh node@node1:/home/node/
            $ scp anaconda-2.3.0-linux-x86_64.sh node@node2:/home/node/
            $ scp anaconda-2.3.0-linux-x86_64.sh node@node3:/home/node/

            # on node1
            $ bash anaconda-2.3.0-linux-x86_64.sh

            # on node2
            $ bash anaconda-2.3.0-linux-x86_64.sh

            # on node3
            $ bash anaconda-2.3.0-linux-x86_64.sh

    1. on dev node

            $ pip install kafaka-python
            $ pip install pyleus

1. kafaka demo

    1. kafaka producer

            def simple_producer():
                '''simple producer'''
                from kafka import SimpleProducer, KafkaClient

                # To send messages synchronously
                kafka = KafkaClient(KAFKA_SERVER)
                producer = SimpleProducer(kafka)

                # Note that the application is responsible for encoding messages to type bytes
                producer.send_messages(b'topic1', b'some message')
                producer.send_messages(b'topic1', b'this method', b'is variadic')
                # Send unicode message
                producer.send_messages(b'topic1', u'你怎么样?'.encode('utf-8'))

    1. kafaka consumer

            def kafka_consumer():
                '''kafka consumer'''
                from kafka import KafkaConsumer

                # To consume messages
                consumer = KafkaConsumer('topic1',
                                         group_id='my-group',
                                         bootstrap_servers=[KAFKA_SERVER])
                for message in consumer:
                    # message value is raw byte string -- decode if necessary!
                    # e.g., for unicode: `message.value.decode('utf-8')`
                    print("%s:%d:%d: key=%s value=%s" % (message.topic, message.partition,
                                                         message.offset, message.key,
                                                         message.value))

1. storm [configuration](https://yelp.github.io/pyleus/configuration.html)

    1. the latter configuration overrides the previous one

            1. /etc/pyleus.conf
            2. ~/.config/pyleus.conf
            3. ~/.pyleus.conf

    1. specify a configuration file

            $ pyleus -c /path/to/pyleus.conf CMD

    1. demo conf

            [storm]
            # path to Storm executable (pyleus will automatically look in PATH)
            storm_cmd_path: /usr/local/bin/storm

            # optional: use -n option of pyleus CLI instead
            nimbus_host: 192.168.1.151

            # optional: use -p option of pyleus CLI instead
            nimbus_port: 6628

            # java options to pass to Storm CLI
            jvm_opts: -Djava.io.tmpdir=/tmp

            [build]
            # PyPI server to use during the build of your topologies
            pypi_index_url: http://pypi.ninjacorp.com/simple/

            # always use system-site-packages for pyleus virtualenvs (default: false)
            system_site_packages: true

            # list of packages to always include in your topologies
            include_packages: foo bar<4.0 baz==0.127

1. [storm demo](https://yelp.github.io/pyleus/tutorial.html)

    1. build a topology

            # on dev
            $ pyleus build /path/to/my_topology.yaml

    1. run a topology locally

            # on dev
            $ pyleus local /path/to/my_topology.jar

            # run the topology on local machine for debugging
            $ pyleus build my_topology/my_topology.yaml
            $ pyleus local --debug my_topology.jar

    1. list all topologies running on a storm cluster

            # on dev
            $ pyleus list -n 192.168.1.151

    1. kill a topology running on a storm cluster

            # on dev
            $ pyleus kill -n 192.168.1.151 my_topology

    1. submit

            # on dev
            $ pyleus submit -n 192.168.1.151 my_topology.jar

    1. directory tree of a simple topology:

            my_topology/
            |-- my_topology/
            | |-- __init__.py
            | |-- dummy_bolt.py
            | |-- dummy_spout.py
            |-- my_topology.yaml
            |-- requirements.txt