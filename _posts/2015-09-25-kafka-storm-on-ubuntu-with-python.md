---
layout: post
title: "kafka storm on ubuntu with python"
description: ""
category: [realtime, python]
tags: [kafka, storm, ubuntu, python, kafka-python, pyleus, nimbus, supervisor, ui, zookeeper]
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
            |    kafka      | |    kafka      | |     kafka     |
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

    1. kafka demo

    1. storm demo

### 1. ubuntu

1. config ubuntu

    1. `ip` of node1

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

            auto eth1
            iface eth1 inet static
            address 10.13.186.251
            netmask 255.255.254.0

    1. `hosts` of node1

            $ sudo pico /etc/hosts
            127.0.0.1   localhost
            #127.0.1.1  node

            # The following lines are desirable for IPv6 capable hosts
            ::1     localhost ip6-localhost ip6-loopback
            ff02::1 ip6-allnodes
            ff02::2 ip6-allrouters

            # at home
            192.168.1.151 node1
            192.168.1.152 node2
            192.168.1.153 node3

            # at work
            # 10.13.186.251 node1
            # 10.13.186.252 node2
            # 10.13.186.253 node3

    1. `hostname` of node1

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

1. check my post [2015 05 26 apache zookeeper getting started]({% post_url 2015-05-26-apache-zookeeper-getting-started %})

### 3. kafka

1. config

    1. server.properties

            # Server Basics #
            broker.id=1

            # Socket Server Settings #
            port=9092
            # hostname the broker willbind to.
            # if not set, the server will bind to all interfaces
            # host.name=192.168.1.151
            num.network.threads=3
            num.io.threads=8
            socket.send.buffer.bytes=102400
            socket.receive.buffer.bytes=102400
            socket.request.max.bytes=104857600

            # Log Basics #
            log.dirs=/opt/app/kafka/logs1
            num.partitions=1
            num.recovery.threads.per.data.dir=1

            # Log Retention Policy #
            log.retention.hours=168
            log.segment.bytes=1073741824
            log.retention.check.interval.ms=300000
            log.cleaner.enable=false

            # Zookeeper #
            zookeeper.connect=192.168.1.151:2181,192.168.1.152:2181,192.168.1.153:2181
            zookeeper.connection.timeout.ms=6000

    1. `host.name=10.13.186.251`

            $ netstat -an | grep 9092
            tcp6       0      0 10.13.186.251:9092      :::*                    LISTEN     
            tcp6       0      0 10.13.186.251:9092      10.13.186.252:34001     ESTABLISHED

    1. `#host.name=10.13.186.251`

            $ netstat -an | grep 9092
            tcp6       0      0 :::9092                 :::*                    LISTEN     
            tcp6       0      0 192.168.1.151:9092      192.168.1.153:40047     ESTABLISHED

1. [usage]((http://www.michael-noll.com/blog/2013/03/13/running-a-multi-broker-apache-kafka-cluster-on-a-single-node/))

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

1. config

    1. storm.yaml

            $ pico storm.yaml
            storm.zookeeper.servers:
                - "10.13.186.251"
                - "10.13.186.252"
                - "10.13.186.253"

            nimbus.host: "10.13.186.251"
            storm.local.dir: "/opt/app/storm"

    1. `storm.local.dir`

            $ ls -R /opt/app/storm/
            /opt/app/storm/:
            nimbus  supervisor

            /opt/app/storm/nimbus:
            inbox

            /opt/app/storm/nimbus/inbox:

            /opt/app/storm/supervisor:
            isupervisor  localstate  tmp

            /opt/app/storm/supervisor/isupervisor:
            1443500644480  1443500644480.version

            /opt/app/storm/supervisor/localstate:
            1443501306564          1443501306723.version  1443501312725
            1443501306564.version  1443501309722          1443501312725.version
            1443501306723          1443501309722.version

            /opt/app/storm/supervisor/tmp:

1. [usage](https://storm.apache.org/documentation/Setting-up-a-Storm-cluster.html)

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
            $ pip install pyleus

            # on node2
            $ bash anaconda-2.3.0-linux-x86_64.sh

            # on node3
            $ bash anaconda-2.3.0-linux-x86_64.sh

    1. on dev node

            $ pip install kafka-python
            $ pip install pyleus

1. kafka demo

    1. kafka producer

            KAFKA_SERVER = '192.168.1.151:9092'

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

    1. kafka consumer

            KAFKA_SERVER = '192.168.1.151:9092'

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

    1. demo conf of `dev`

            $ cd ~ && pico .pyleus.conf
            [storm]
            # path to Storm executable (pyleus will automatically look in PATH)
            storm_cmd_path: /usr/local/bin/storm

            # optional: use -n option of pyleus CLI instead
            nimbus_host: 192.168.1.151
            #nimbus_host: 10.13.186.251

            # optional: use -p option of pyleus CLI instead
            nimbus_port: 6627

            # java options to pass to Storm CLI
            jvm_opts: -Djava.io.tmpdir=/tmp

            [build]
            # PyPI server to use during the build of your topologies
            pypi_index_url: http://pypi.ninjacorp.com/simple/

            # always use system-site-packages for pyleus virtualenvs (default: false)
            system_site_packages: false

    1. demo conf of `node1`

            $ cd ~ && pico .pyleus.conf
            [storm]
            # path to Storm executable (pyleus will automatically look in PATH)
            storm_cmd_path: /opt/storm/bin/storm

            # optional: use -n option of pyleus CLI instead
            nimbus_host: 192.168.1.151
            #nimbus_host: 10.13.186.251

            # optional: use -p option of pyleus CLI instead
            nimbus_port: 6627

            # java options to pass to Storm CLI
            jvm_opts: -Djava.io.tmpdir=/tmp

            [build]
            # PyPI server to use during the build of your topologies
            pypi_index_url: http://pypi.ninjacorp.com/simple/

            # always use system-site-packages for pyleus virtualenvs (default: false)
            system_site_packages: false

    1. about [system_site_packages: false](https://yelp.github.io/pyleus/install.html)

            You do NOT need to install pyleus on your Storm cluster.

            However, if you are going to use system_site_packages: true in your config file, you should be aware that the environment of your Storm nodes needs to match the one on the machine used for building the topology. This means you actually have to install pyleus on your Storm cluster in this case.

1. [storm demo](https://yelp.github.io/pyleus/tutorial.html)

    1. build a topology

            # on dev
            $ cd ~/word_count
            $ pyleus build word_count.yaml

    1. run a topology locally

            # on dev
            $ cd ~/word_count
            $ pyleus local word_count.jar

            # run the topology on local machine for debugging
            $ pyleus local --debug word_count.jar

    1. list all topologies running on a storm cluster

            # on dev
            $ pyleus list

    1. submit

            # on dev
            $ pyleus submit word_count.jar

    1. kill a topology running on a storm cluster

            # on dev
            $ pyleus kill word_count

    1. directory tree of a simple topology:

            word_count/
            ├── word_count
            │   ├── __init__.py
            │   ├── count_words.py
            │   ├── line_spout.py
            │   ├── log_results.py
            │   └── split_words.py
            └── word_count.yaml

1. put all together

    1. data flow chart

            +------------------+
            | producer.demo.py |
            +------------------+
                      ↓         
            +------------------+
            | kafka cluster    |
            +------------------+
                      ↓         
            +------------------+
            | kafka-spout      |
            +------------------+
                      ↓         
            +------------------+
            | split-words.py   |
            +------------------+
                      ↓         
            +------------------+
            | count-words.py   |
            +------------------+
                      ↓         
            +------------------+
            | log-results.py   |
            +------------------+
                      ↓         
            +------------------+
            | redis            |
            +------------------+


    1. local test flow

            +------------------+
            | start zookeeper  |
            +------------------+
                      ↓         
            +------------------+
            | start kafka      |
            +------------------+
                      ↓         
            +------------------+
            | start storm      |
            +------------------+
                      ↓         
            +------------------+
            | pyleus local     |
            +------------------+
                      ↓         
            +------------------+
            | run producer * x |
            +------------------+
                      ↓         
            +------------------+
            | check dir /tmp   |
            | check redis      |
            +------------------+

    1. process

            +----------------+   
            | sentence spout |   {"sentence": "i am learning kafka storm redis"}
            +----------------+   
                    ↓            
            +----------------+   
            | split bolt     |   {"word" : "i"} {"word" : "am"} {"word" : "learning"}
            +----------------+   
                    ↓            
            +----------------+   
            | count bolt     |   {"word" : "i", "count" : 1} {"word" : "am", "count" : 1}
            +----------------+   
                    ↓            
            +----------------+   
            | log bolt       |   write to log and to redis
            +----------------+   

### fixed

1. local mode on dev

    1. error

            2015-09-28T16:10:32.987+0800 b.s.d.worker [ERROR] Error on initialization of server mk-worker
            java.io.FileNotFoundException: File '/opt/app/storm/supervisor/stormdist/word_count-2-1443427681/stormconf.ser' does not exist
                at org.apache.commons.io.FileUtils.openInputStream(FileUtils.java:299) ~[commons-io-2.4.jar:2.4]
                at org.apache.commons.io.FileUtils.readFileToByteArray(FileUtils.java:1763) ~[commons-io-2.4.jar:2.4]
                at backtype.storm.config$read_supervisor_storm_conf.invoke(config.clj:212) ~[storm-core-0.9.5.jar:0.9.5]
                at backtype.storm.daemon.worker$worker_data.invoke(worker.clj:184) ~[storm-core-0.9.5.jar:0.9.5]
                at backtype.storm.daemon.worker$fn__6959$exec_fn__1103__auto____6960.invoke(worker.clj:400) ~[storm-core-0.9.5.jar:0.9.5]
                at clojure.lang.AFn.applyToHelper(AFn.java:185) [clojure-1.5.1.jar:na]
                at clojure.lang.AFn.applyTo(AFn.java:151) [clojure-1.5.1.jar:na]
                at clojure.core$apply.invoke(core.clj:617) ~[clojure-1.5.1.jar:na]
                at backtype.storm.daemon.worker$fn__6959$mk_worker__7015.doInvoke(worker.clj:391) [storm-core-0.9.5.jar:0.9.5]
                at clojure.lang.RestFn.invoke(RestFn.java:512) [clojure-1.5.1.jar:na]
                at backtype.storm.daemon.worker$_main.invoke(worker.clj:502) [storm-core-0.9.5.jar:0.9.5]
                at clojure.lang.AFn.applyToHelper(AFn.java:172) [clojure-1.5.1.jar:na]
                at clojure.lang.AFn.applyTo(AFn.java:151) [clojure-1.5.1.jar:na]
                at backtype.storm.daemon.worker.main(Unknown Source) [storm-core-0.9.5.jar:0.9.5]
            2015-09-28T16:10:33.022+0800 b.s.util [ERROR] Halting process: ("Error on initialization")
            java.lang.RuntimeException: ("Error on initialization")
                at backtype.storm.util$exit_process_BANG_.doInvoke(util.clj:325) [storm-core-0.9.5.jar:0.9.5]
                at clojure.lang.RestFn.invoke(RestFn.java:423) [clojure-1.5.1.jar:na]
                at backtype.storm.daemon.worker$fn__6959$mk_worker__7015.doInvoke(worker.clj:391) [storm-core-0.9.5.jar:0.9.5]
                at clojure.lang.RestFn.invoke(RestFn.java:512) [clojure-1.5.1.jar:na]
                at backtype.storm.daemon.worker$_main.invoke(worker.clj:502) [storm-core-0.9.5.jar:0.9.5]
                at clojure.lang.AFn.applyToHelper(AFn.java:172) [clojure-1.5.1.jar:na]
                at clojure.lang.AFn.applyTo(AFn.java:151) [clojure-1.5.1.jar:na]
                at backtype.storm.daemon.worker.main(Unknown Source) [storm-core-0.9.5.jar:0.9.5]

    1. solution

            # 1. stop kafka storm zookeeper
            # 2. delete `storm's` folder `storm.localdir` contents
            # 3. delete `zookeeper's` folder `dataDir` and `dataLogDir`
            # 3. start zookeeper kafka storm

1. local mode on dev

    1. error

            java.lang.RuntimeException: Error when launching multilang subprocess bash: pyleus_venv/bin/python: cannot execute binary file: Exec format error at backtype.storm.utils.ShellProcess.launch(ShellProcess.java:66) at backtype.storm.task.ShellBolt.prepare(ShellBolt.java:117) at backtype.storm.daemon.executor$fn__6647$fn__6659.invoke(executor.clj:692) at backtype.storm.util$async_loop$fn__459.invoke(util.clj:461) at clojure.lang.AFn.run(AFn.java:24) at java.lang.Thread.run(Thread.java:745) Caused by: java.io.IOException: Stream closed at java.lang.ProcessBuilder$NullOutputStream.write(ProcessBuilder.java:434) at java.io.OutputStream.write(OutputStream.java:116) at java.io.BufferedOutputStream.flushBuffer(BufferedOutputStream.java:82) at java.io.BufferedOutputStream.flush(BufferedOutputStream.java:140) at java.io.DataOutputStream.flush(DataOutputStream.java:123) at com.yelp.pyleus.serializer.MessagePackSerializer.writeMessage(MessagePackSerializer.java:215) at com.yelp.pyleus.serializer.MessagePackSerializer.connect(MessagePackSerializer.java:65) at backtype.storm.utils.ShellProcess.launch(ShellProcess.java:64) ... 5 more

    1. solution

            # not found yet
            # https://yelp.github.io/pyleus/install.html

1. local mode on node1

    1. error

            java.lang.RuntimeException: Error when launching multilang subprocess
            Traceback (most recent call last):
              File "/home/node/anaconda/lib/python2.7/runpy.py", line 162, in _run_module_as_main
                "__main__", fname, loader, pkg_name)
              File "/home/node/anaconda/lib/python2.7/runpy.py", line 72, in _run_code
                exec code in run_globals
              File "/tmp/e50a8af4-bec0-41fa-91cd-22aabd2f1c1b/supervisor/stormdist/word_count-1-1443861558/resources/word_count/log_results.py", line 7, in <module>
                from pyleus.storm import SimpleBolt
              File "/home/node/anaconda/lib/python2.7/site-packages/pyleus/__init__.py", line 3, in <module>
                import pkg_resources
              File "/tmp/e50a8af4-bec0-41fa-91cd-22aabd2f1c1b/supervisor/stormdist/word_count-1-1443861558/resources/pyleus_venv/lib/python2.7/site-packages/pkg_resources/__init__.py", line 37, in <module>
                import email.parser
              File "/home/node/anaconda/lib/python2.7/email/parser.py", line 12, in <module>
                from email.feedparser import FeedParser
              File "/home/node/anaconda/lib/python2.7/email/feedparser.py", line 27, in <module>
                from email import message
              File "/home/node/anaconda/lib/python2.7/email/message.py", line 16, in <module>
                import email.charset
              File "/home/node/anaconda/lib/python2.7/email/charset.py", line 13, in <module>
                import email.base64mime
              File "/home/node/anaconda/lib/python2.7/email/base64mime.py", line 40, in <module>
                from email.utils import fix_eols
              File "/home/node/anaconda/lib/python2.7/email/utils.py", line 27, in <module>
                import random
              File "/home/node/anaconda/lib/python2.7/random.py", line 49, in <module>
                import hashlib as _hashlib
              File "/home/node/anaconda/lib/python2.7/hashlib.py", line 138, in <module>
                _hashlib.openssl_md_meth_names)
            AttributeError: 'module' object has no attribute 'openssl_md_meth_names'

    1. [solution](http://askubuntu.com/questions/575598/python-attributeerror-with-hashlib-no-such-attribute-openssl-md-meth-names)

            $ cd / && find . | grep hashlib
            ./usr/lib/python3.4/lib-dynload/_hashlib.cpython-34m-x86_64-linux-gnu.so
            ./usr/lib/python3.4/hashlib.py
            ./usr/lib/python3.4/__pycache__/hashlib.cpython-34.pyc
            ./usr/lib/python2.7/lib-dynload/_hashlib.x86_64-linux-gnu.so
            ./usr/lib/python2.7/hashlib.py
            ./usr/lib/python2.7/hashlib.pyc
            ...

            $ sudo rm -f /usr/lib/python2.7/lib-dynload/_hashlib.x86_64-linux-gnu.so
