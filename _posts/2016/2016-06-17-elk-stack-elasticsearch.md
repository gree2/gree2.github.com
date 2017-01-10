---
layout: post
title: "elk stack elasticsearch"
description: ""
category: [bigdata]
tags: [elk, elasticsearch, logstash, kibana]
---
{% include JB/setup %}


### info

1. [elasticsearch](https://www.elastic.co/products/elasticsearch)

1. [downloads](https://www.elastic.co/downloads/elasticsearch)

1. [documentation](https://www.elastic.co/guide/index.html)

1. install steps

    1. download

            #### AAA install with apt-get
            # 1. import pgp key
            # download and install the public signing key
            $ wget -qO https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

            # 2. installing from apt repository
            # 2.1 install `apt-transport-https` before proceeding
            $ sudo apt-get install apt-transport-https

            # 2.2 save repo definition
            $ echo "deb https://packages.elastic.co/elasticsearch/5.x/debian stable main" | \
            sudo tee -a /etc/apt/sources.list.d/elasticsearch-5.x.list

            # 2.3 install
            $ sudo apt-get update && sudo apt-get install elasticsearch

            #### BBB install manually
            $ wget https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/deb/elasticsearch/5.0.0-alpha3/elasticsearch-5.0.0-alpha3.deb
            $ sha1sum elasticsearch-5.0.0-alpha3.deb 
            $ sudo dpkg -i elasticsearch-5.0.0-alpha3.deb

            #### CCC install from zip
            $ curl -O https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/zip/elasticsearch/5.0.0-alpha3/elasticsearch-5.0.0-alpha3.zip
            $ unzip elasticsearch-5.0.0-alpha3.zip
            $ cd elasticsearch-5.0.0-alpha3

    1. run elasticsearch

            # 1. on unix
            $ bin/elasticsearch

            # 2. on windows
            > bin/elasticsearch

    1. run
            $ curl -X GET http://localhost:9200/
            $ curl 'http://localhost:9200/?pretty'

            # shutdown elasticsearch
            $ curl -XPOST 'http://localhost:9200/_shutdown'

1. config

    1. network address

            network.host: 127.0.0.1

    1. path

            path:
                logs: /var/log/elasticsearch
                data: /var/data/elasticsearch

    1. cluster name

            cluster:
                name: <name of cluster>

    1. node name

            node:
                name: <name of node>

1. plugins

    1. install plugin

            $ bin/plugin -install lmenezes/elasticsearch-kopf

            # webui
            # http://localhost:9200/_plugin/kopf/

1. curl

    1. info

            $ curl 'http://192.168.99.100:9200/?pretty'
            {
                "name" : "Ellie Phimster",
                "cluster_name" : "elasticsearch",
                "version" : {
                    "number" : "2.3.3",
                    "build_hash" : "218bdf10790eef486ff2c41a3df5cfa32dadcfde",
                    "build_timestamp" : "2016-05-17T15:40:04Z",
                    "build_snapshot" : false,
                    "lucene_version" : "5.5.0"
                },
                "tagline" : "You Know, for Search"
            }

    1. shutdown

            $ curl -XPOST 'http://192.168.99.100:9200/_shutdown'

    1. search

            $ curl –XGET 'http://192.168.99.100:9200/logstash-2014.08.04/_search?pretty'

    1. indices

            $ curl –XGET 'http:192.168.99.100:9200/_cat/indices?v'

    1. node
    
            $ curl –XGET 'http://192.168.99.100:9200/_cat/nodes?v'

    1. health

            $ curl -XGET 'http://192.168.99.100:9200/_cluster/health?pretty=true'
            {
                "cluster_name" : "elasticsearch",
                "status" : "yellow",
                "timed_out" : false,
                "number_of_nodes" : 1,
                "number_of_data_nodes" : 1,
                "active_primary_shards" : 5,
                "active_shards" : 5,
                "relocating_shards" : 0,
                "initializing_shards" : 0,
                "unassigned_shards" : 5,
                "delayed_unassigned_shards" : 0,
                "number_of_pending_tasks" : 0,
                "number_of_in_flight_fetch" : 0,
                "task_max_waiting_in_queue_millis" : 0,
                "active_shards_percent_as_number" : 50.0
            }

            $ curl -XGET 'http://192.168.99.100:9200/_cluster/health?level=cluster&pretty=true'
            $ curl -XGET 'http://192.168.99.100:9200/_cluster/health?level=shards&pretty=true'
            $ curl -XGET 'http://192.168.99.100:9200/_cluster/health?level=indices&pretty=true'

### docker-elasticsearch

1. [git repo](https://github.com/gree2/hello-docker/tree/master/docker-elk/docker-elasticsearch)

1. don't run elasticsearch as root.

    1. exception

            $ docker run -it --name gelasticsearch gree2/elasticsearch
            Exception in thread "main" java.lang.RuntimeException: don't run elasticsearch as root.
                    at org.elasticsearch.bootstrap.Bootstrap.initializeNatives(Bootstrap.java:93)
                    at org.elasticsearch.bootstrap.Bootstrap.setup(Bootstrap.java:144)
                    at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:270)
                    at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:35)
            Refer to the log for complete error details.

    1. [switching users inside docker image to a non root user](http://stackoverflow.com/questions/24549746/switching-users-inside-docker-image-to-a-non-root-user)

            RUN whoami
            USER test
            RUN whoami

            # you should see `root` then `test`

1. nosuchfileexception /usr/share/elasticsearch/config

    1. exception

            Exception in thread "main" ElasticsearchException[Failed to load logging configuration]; nested: NoSuchFileException[/usr/share/elasticsearch/config];
            Likely root cause: java.nio.file.NoSuchFileException: /usr/share/elasticsearch/config
                at sun.nio.fs.UnixException.translateToIOException(UnixException.java:86)
                at sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:102)
                at sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:107)
                at sun.nio.fs.UnixFileAttributeViews$Basic.readAttributes(UnixFileAttributeViews.java:55)
                at sun.nio.fs.UnixFileSystemProvider.readAttributes(UnixFileSystemProvider.java:144)
                at sun.nio.fs.LinuxFileSystemProvider.readAttributes(LinuxFileSystemProvider.java:97)
                at java.nio.file.Files.readAttributes(Files.java:1686)
                at java.nio.file.FileTreeWalker.walk(FileTreeWalker.java:109)
                at java.nio.file.FileTreeWalker.walk(FileTreeWalker.java:69)
                at java.nio.file.Files.walkFileTree(Files.java:2602)
                at org.elasticsearch.common.logging.log4j.LogConfigurator.resolveConfig(LogConfigurator.java:142)
                at org.elasticsearch.common.logging.log4j.LogConfigurator.configure(LogConfigurator.java:103)
                at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:243)
                at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:35)
            Refer to the log for complete error details.

    1. [run es_bin as es_usr](https://discuss.elastic.co/t/issues-installing-starting-es-2-1-0-when-installed-from-a-repository/36320)

            #         es_usr        es_bin        es_conf
            $ sudo -u elasticsearch elasticsearch -Des.path.conf=/etc/elasticsearch
