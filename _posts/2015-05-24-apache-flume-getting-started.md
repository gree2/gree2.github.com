---
layout: post
title: "apache flume getting started"
description: ""
category: [apache]
tags: [flume]
---
{% include JB/setup %}


### what is flume ng

* flume ng (next generation) is a huge departure from flume og (original generation)

    * channels are pluggable and dictate durability

    * in-memory channel file-based channel

    * we call physical nodes agents and agents run zero or more sources and sinks

    * no master and no zookeeper dependency

    * everything is a plugin

### just want to run flume

1. [download](http://flume.apache.org/download.html)

1. decompress

            $ tar -zxvf apache-flume-*.tar.gz
            $ cd apache-flume...

1. create your own properties file based on working template

            $ cp conf/flume-conf.properties.template conf/flume.conf

1. (optional) create your `flume-env.sh`

            $ cp conf/flume-env.sh.template conf/flume-env.sh

1. configure and run flume ng

            # after you've configured flume ng (see below) you can run it
            $ bin/flume-ng

### configuration

* flume uses java property file based configuration format

* use `-f <file>` to tell flume which file to use

* example of `conf/flume.conf`

            # define a memory channel called ch1 on agent1
            agent1.channels.ch1.type = memory

            # define an avro source called avro-source1 on agent1
            # bind to 0.0.0.0:41414
            # connect it to channel ch1
            agent1.sources.avro-source1.channels = ch1
            agent1.sources.avro-source1.type = avro
            agent1.sources.avro-source1.bind = 0.0.0.0
            agent1.sources.avro-source1.port = 41414

            # define a logger sink that simply logs all events it receives
            # connect it to the other end of the same channel
            agent1.sinks.log-sink1.channel = ch1
            agnet1.sinks.log-sink1.type = logger

            # define all components
            # tell agent1 which ones we want to activate
            agent1.channels = ch1
            agent1.sources = avro-source1
            agent1.sinks = log-sink1

* channel type

            memory, file, jdbc, recoverablememory
            org.apache.flume.channel.PseudoTxnMemoryChannel

* source type

            avro, exec, netcat, seq
            org.apache.flume.source.StressSource
            syslogtcp, syslogudp
            org.apache.flume.source.avroLegacy.AvroLegacySource
            org.apache.flume.source.thriftLegacy.ThrigtLegacySource
            org.apache.flume.source.scribe.ScribeSource

* sink type

            hdfs, logger, avro, file_roll, irc, null
            org.apache.flume.sink.hbase.HBaseSink
            org.apache.flume.sink.hbase.AsyncHBaseSink

* channelselector type

            replicating, multiplexing, (custom type)

* sinkprocessor type

            default, failover, load_balance

* interceptor$builder type

            host, timestamp, static, regex_filter

* eventserializer$builder type

            text, avro_event
            org.apache.flume.sink.hbase.SimpleHbaseEventSerializer
            org.apache.flume.sink.hbase.SimpleAsyncHbaseEventSerializer
            org.apache.flume.sink.hbase.RegexHbaseEventSerializer

* to start flume server using the `flume.conf` above

            $ bin/flume-ng agent --conf ./conf/ -f conf/flume.conf -Dflume.root.logger=DEBUG,confole -n agent1
            # agent name specified by `-n agent1`
            # must match a agent name give in
            # `-f conf/flume.conf`

### flume-ng global options

* --conf,-c <conf>

            use configs in <conf> directory

* --classpath,-C <cp>

            append to the classpath

* --dryrun,-d

            do not actually start flume, just print the command

* -Dproperty=value

            set a jdk system property value

### flume-ng agent options

* --conf-file,-f <file>

            indicates which configuration file you want to run with (required)

* --name,-n <agentname>

            indicates the name of agent on which we're running (required)

### flume-ng avro-client options

* --host,-H <hostname>

            specifies the hostname of the flume agent (may be localhost)

* --port,-p <port>

            specified the port on which the avro source is listening

* --filename,-F <filename>

            send each line of <filename> to flume (optional)

* --headerFile,-F <file>

            header file containning headers as key/value pairs on each new line

            # avro client treats each line (\n, \r, \r\n) as an event

### in a new window type

            $ bin/flume-ng avro-client --conf conf -H localhost -p 41414 -F /etc/passwd -Dflume.root.logger=DEBUG,console