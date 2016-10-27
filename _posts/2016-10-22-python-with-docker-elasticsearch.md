---
layout: post
title: "python with docker elasticsearch"
description: ""
category: [docker, python]
tags: [es, python, docker, elasticsearch]
---
{% include JB/setup %}


### elasticsearch

1. pull and run

    1. pull

            # docker pull elasticsearch

    1. simply

            # docker run -d -p 9200:9200 9300:9300 --name ges elasticsearch

    1. pass flags

            # docker run -d elasticsearch -Des.node.name="TestNode"

    1. provide config

            # docker run -d -v "$PWD/config":/usr/share/elasticsearch/config elasticsearch

    1. persist data

            # docker run -d -v "$PWD/esdata":/usr/share/elasticsearch/data elasticsearch

1. browse [http://localhost:9200/](http://localhost:9200/)

    1. output

            {
              "name" : "Conrad Josten",
              "cluster_name" : "elasticsearch",
              "cluster_uuid" : "rFYCofG9QQmEDLLzdVSsOw",
              "version" : {
                "number" : "2.4.1",
                "build_hash" : "c67dc32e24162035d18d6fe1e952c4cbcbe79d16",
                "build_timestamp" : "2016-09-27T18:57:55Z",
                "build_snapshot" : false,
                "lucene_version" : "5.5.2"
              },
              "tagline" : "You Know, for Search"
            }

### python with elasticsearch

1. install package

    1. run

            # pip install elasticsearch

    1. [demo code](http://elasticsearch-py.readthedocs.io/en/master/)

            from datetime import datetime
            from elasticsearch import Elasticsearch
            es = Elasticsearch()

            doc = {
                'author': 'kimchy',
                'text': 'Elasticsearch: cool. bonsai cool.',
                'timestamp': datetime.now(),
            }
            res = es.index(index="test-index", doc_type='tweet', id=1, body=doc)
            print(res['created'])

            res = es.get(index="test-index", doc_type='tweet', id=1)
            print(res['_source'])

            es.indices.refresh(index="test-index")
            res = es.search(index="test-index", body={"query": {"match_all": {}}})
            print("Got %d Hits:" % res['hits']['total'])
            for hit in res['hits']['hits']:
                print("%(timestamp)s %(author)s: %(text)s" % hit["_source"])

    1. output

            True
            {u'text': u'Elasticsearch: cool. bonsai cool.', u'author': u'kimchy', u'timestamp': u'2016-10-22T15:56:21.202901'}
            Got 1 Hits:
            2016-10-22T15:56:21.202901 kimchy: Elasticsearch: cool. bonsai cool.
