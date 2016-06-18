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

            network:
                host: 127.0.0.1

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

### docker-elasticsearch

1. [git repo](https://github.com/gree2/hello-docker/tree/master/docker-elk/docker-elasticsearch)

1. fixed

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
