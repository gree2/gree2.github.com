---
layout: post
title: "apache flume getting started"
description: ""
category: [bigdata]
tags: [flume, avro, netcat, source, sink, channel]
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

### demo

1. demo 1

    1. example of `conf/avro.flume.conf`

            agent-av.sources = avro-source
            agent-av.channels = memory-channel
            agent-av.sinks = logger-sink

            agent-av.sources.avro-source.type = avro
            agent-av.sources.avro-source.channels = memory-channel
            agent-av.sources.avro-source.bind = 0.0.0.0
            agent-av.sources.avro-source.port = 41414

            agent-av.channels.memory-channel.type = memory
            agent-av.channels.memory-channel.capacity = 100
            agent-nc.channels.memory-channel.transactionCapacity = 100

            agent-av.sinks.logger-sink.type = logger
            agent-av.sinks.logger-sink.channel = memory-channel

    1. to start flume server using the `flume.conf` above

            # agent name specified by `-n agent-av`
            # must match a agent name give in `-f conf/flume.conf`
            $ flume-ng agent --conf ./conf/ -f conf/avro.flume.conf -Dflume.root.logger=DEBUG,confole -n agent-av

    1. in a new window type

            $ flume-ng avro-client --conf conf -H localhost -p 41414 -F /etc/passwd -Dflume.root.logger=DEBUG,console

1. demo 2

    1. example of `conf/netcat.flume.conf`

            agent-nc.sources = netcat-source
            agent-nc.channels = memory-channel
            agent-nc.sinks = logger-sink

            agent-nc.sources.netcat-source.type = netcat
            agent-nc.sources.netcat-source.channels = memory-channel
            agent-nc.sources.netcat-source.bind = localhost
            agent-nc.sources.netcat-source.port = 44444

            agent-nc.channels.memory-channel.type = memory
            agent-nc.channels.memory-channel.capacity = 1000
            agent-nc.channels.memory-channel.transactionCapacity = 100

            agent-nc.sinks.logger-sink.type = logger
            agent-nc.sinks.logger-sink.channel = memory-channel

    1. to start flume server

            $ flume-ng agent --conf ./conf/ -f conf/netcat.flume.conf -Dflume.root.logger=DEBUG,confole -n agent-nc

    1. in a new window type

            $ telnet localhost 44444
            Trying ::1...
            telnet: connect to address ::1: Connection refused
            Trying 127.0.0.1...
            Connected to localhost.
            Escape character is '^]'.
            hello
            OK
            are you ok

### channel, sources, sinks type

1. channel type

            memory, file, jdbc, recoverablememory
            org.apache.flume.channel.PseudoTxnMemoryChannel

1. source type

            avro, exec, netcat, seq
            org.apache.flume.source.StressSource
            syslogtcp, syslogudp
            org.apache.flume.source.avroLegacy.AvroLegacySource
            org.apache.flume.source.thriftLegacy.ThrigtLegacySource
            org.apache.flume.source.scribe.ScribeSource

1. sink type

            hdfs, logger, avro, file_roll, irc, null
            org.apache.flume.sink.hbase.HBaseSink
            org.apache.flume.sink.hbase.AsyncHBaseSink

1. channelselector type

            replicating, multiplexing, (custom type)

1. sinkprocessor type

            default, failover, load_balance

1. interceptor$builder type

            host, timestamp, static, regex_filter

1. eventserializer$builder type

            text, avro_event
            org.apache.flume.sink.hbase.SimpleHbaseEventSerializer
            org.apache.flume.sink.hbase.SimpleAsyncHbaseEventSerializer
            org.apache.flume.sink.hbase.RegexHbaseEventSerializer

### flume-ng options

1. global

    1. --conf,-c `conf`

            use configs in `conf` directory

    1. --classpath,-C `cp`

            append to the classpath

    1. --dryrun,-d

            do not actually start flume, just print the command

    1. -Dproperty=value

            set a jdk system property value

1. agent

    1. --conf-file,-f `file`

            indicates which configuration file you want to run with (required)

    1. --name,-n `agentname`

            indicates the name of agent on which we're running (required)

1. avro-client

    1. --host,-H `hostname`

            specifies the hostname of the flume agent (may be localhost)

    1. --port,-p `port`

            specified the port on which the avro source is listening

    1. --filename,-F `filename`

            send each line of `filename` to flume (optional)

    1. --headerFile,-F `file`

            header file containning headers as key/value pairs on each new line

            # avro client treats each line (\n, \r, \r\n) as an event