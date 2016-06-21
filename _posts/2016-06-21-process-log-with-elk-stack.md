---
layout: post
title: "process log with elk stack"
description: ""
category: [bigdata]
tags: [elk, log]
---
{% include JB/setup %}


### setup container

1. elasticsearch

    1. [Dockerfile](https://github.com/gree2/hello-docker/blob/master/docker-elk/docker-elasticsearch/Dockerfile)

            # 1. install from gz

            # 2. config
            replace default `network.host` setup
            with `network.host: 0.0.0.0`

            # 3. add group and user
            # 4. run as user

    1. run

            $docker run -it --name gelasticsearch -p 9200:9200 -p 9300:9300 gree2/elasticsearch
            # this follow 3 lines were output
            # when you type `hello this is from logstash` in container glogstash's tty
            [2016-06-21 01:25:21,832][INFO ][cluster.metadata         ] [Ellie Phimster] [logstash-2016.06.21] creating index, cause [auto(bulk api)], templates [logstash], shards [5]/[1], mappings [logs, _default_]
            [2016-06-21 01:25:21,950][INFO ][cluster.routing.allocation] [Ellie Phimster] Cluster health status changed from [RED] to [YELLOW] (reason: [shards started [[logstash-2016.06.21][4]] ...]).
            [2016-06-21 01:25:21,975][INFO ][cluster.metadata         ] [Ellie Phimster] [logstash-2016.06.21] update_mapping [logs]

1. logstash

    1. [Dockerfile](https://github.com/gree2/hello-docker/blob/master/docker-elk/docker-logstash/Dockerfile)

            # 1. install with apt
            # 2. default run logstash -e 'input { stdin { } } output{ stdout{ } }'

    1. [run use link](https://github.com/gree2/hello-docker/blob/master/docker-elk/docker-logstash/link.sh)

            $ docker run -it --rm --link gelasticsearch --name glogstash gree2/logstash logstash -e 'input { stdin { } } output{ elasticsearch { } }'

    1. [run use hosts](https://github.com/gree2/hello-docker/blob/master/docker-elk/docker-logstash/run.sh)

            $ docker run -it --rm --name glogstash gree2/logstash logstash -e 'input { stdin { } } output{ elasticsearch { hosts => ["192.168.99.100:9200"] } }'
            Settings: Default pipeline workers: 1
            Pipeline main started
            hello this is from logstash
            ^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. kibana

    1. [Dockerfile](https://github.com/gree2/hello-docker/blob/master/docker-elk/docker-kibana/Dockerfile)

            # 1. install with apt
            # 2. replace elasticsearch.url
            # 3. run kibana

    1. [run use link](https://github.com/gree2/hello-docker/blob/master/docker-elk/docker-kibana/link.sh)

            $ docker run -it --rm --link gelasticsearch --name gkibana -p 5601:5601 gree2/kibana

### code

1. [input dataset google daily quotes price 2006-01-01~2016-06-21](http://finance.yahoo.com/q/hp?s=GOOG&a=00&b=1&c=2006&d=05&e=21&f=2016&g=d)

1. logstash

    1. config file

            
            $ pico goog.conf
            input {
                file {
                    path => "/opt/logstash/input/goog.csv"
                    start_position => "beginning"
                }
            }

            filter {
                csv {
                    columns => ["date_of_record","open","high","low","close","volumn","adj_close"]
                    separator => ","
                }
                date {
                    match => ["date_of_record", "yyyy-MM-dd"]
                }
                mutate {
                    convert => {
                        "open" => "float",
                        "high" => "float",
                        "low" => "float",
                        "close" => "float",
                        "volume" => "float",
                        "adj_close" => "float",
                    }
                }
            }

            output {
                elasticsearch {
                    hosts => ["192.168.99.100:9200"]
                }
            }

    1. run

            $ logstash -f goog.conf

### fixed

1. no living connections

    1. test from kibana container

            $ ./link.sh
            root@ab087250f904:/# kibana
            log   [01:41:38.574] [info][status][plugin:kibana] Status changed from uninitialized to green - Ready
            log   [01:41:38.603] [info][status][plugin:elasticsearch] Status changed from uninitialized to yellow - Waiting for Elasticsearch
            log   [01:41:38.614] [info][status][plugin:kbn_vislib_vis_types] Status changed from uninitialized to green - Ready
            log   [01:41:38.618] [info][status][plugin:markdown_vis] Status changed from uninitialized to green - Ready
            log   [01:41:38.633] [info][status][plugin:metric_vis] Status changed from uninitialized to green - Ready
            log   [01:41:38.638] [error][elasticsearch] Request error, retrying -- connect ECONNREFUSED 127.0.0.1:9200
            log   [01:41:38.650] [info][status][plugin:spyModes] Status changed from uninitialized to green - Ready
            log   [01:41:38.653] [info][status][plugin:statusPage] Status changed from uninitialized to green - Ready
            log   [01:41:38.656] [warning][elasticsearch] Unable to revive connection: http://localhost:9200/
            log   [01:41:38.657] [warning][elasticsearch] No living connections
            log   [01:41:38.661] [error][status][plugin:elasticsearch] Status changed from yellow to red - Unable to connect to Elasticsearch at http://localhost:9200.
            log   [01:41:38.662] [info][status][plugin:table_vis] Status changed from uninitialized to green - Ready
            log   [01:41:38.673] [info][listening] Server running at http://0.0.0.0:5601
            log   [01:41:41.168] [warning][elasticsearch] Unable to revive connection: http://localhost:9200/
            log   [01:41:41.170] [warning][elasticsearch] No living connections

    1. fix with config

            # in Dockerfile
            # replace default elasticsearch.url
            RUN sed -i 's|# elasticsearch.url: "http://localhost:9200"|elasticsearch.url: "http://192.168.99.100:9200"|g' $KIBANA_HOME/config/kibana.yml

1. field data loading is forbidden on [message]

    1. test from kibana webui line chart

            [2016-06-21 02:36:31,214][DEBUG][action.search            ] [Ellie Phimster] All shards failed for phase: [query]
            RemoteTransportException[[Ellie Phimster][172.17.0.2:9300][indices:data/read/search[phase/query]]]; nested: SearchParseException[failed to parse search source [{"size":0,"query":{"filtered":{"query":{"query_string":{"analyze_wildcard":true,"query":"*"}},"filter":{"bool":{"must":[{"range":{"@timestamp":{"gte":1466474700000,"lte":1466474730000,"format":"epoch_millis"}}}],"must_not":[]}}}},"aggs":{"3":{"terms":{"field":"message","size":20,"order":{"_count":"desc"}}}}}]]; nested: IllegalStateException[Field data loading is forbidden on [message]];
            Caused by: SearchParseException[failed to parse search source [{"size":0,"query":{"filtered":{"query":{"query_string":{"analyze_wildcard":true,"query":"*"}},"filter":{"bool":{"must":[{"range":{"@times tamp":{"gte":1466474700000,"lte":1466474730000,"format":"epoch_millis"}}}],"must_not":[]}}}},"aggs":{"3":{"terms":{"field":"message","size":20,"order":{"_count":"desc"}}}}}]]; nested: IllegalStateException[Field data loading is forbidden on [message]]; at org.elasticsearch.search.SearchService.parseSource(SearchService.java:855) at org.elasticsearch.search.SearchService.createContext(SearchService.java:654)
                    at org.elasticsearch.search.SearchService.createAndPutContext(SearchService.java:620)
                    at org.elasticsearch.search.SearchService.executeQueryPhase(SearchService.java:371)
                    at org.elasticsearch.search.action.SearchServiceTransportAction$SearchQueryTransportHandler.messageReceived(SearchServiceTransportAction.java:368)
                    at org.elasticsearch.search.action.SearchServiceTransportAction$SearchQueryTransportHandler.messageReceived(SearchServiceTransportAction.java:365)
                    at org.elasticsearch.transport.TransportRequestHandler.messageReceived(TransportRequestHandler.java:33)
                    at org.elasticsearch.transport.RequestHandlerRegistry.processMessageReceived(RequestHandlerRegistry.java:75)
                    at org.elasticsearch.transport.TransportService$4.doRun(TransportService.java:376)
                    at org.elasticsearch.common.util.concurrent.AbstractRunnable.run(AbstractRunnable.java:37)
                    at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
                    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
                    at java.lang.Thread.run(Thread.java:745)
            Caused by: java.lang.IllegalStateException: Field data loading is forbidden on [message]
                    at org.elasticsearch.index.fielddata.IndexFieldDataService$1.build(IndexFieldDataService.java:74)
                    at org.elasticsearch.index.fielddata.IndexFieldDataService.getForField(IndexFieldDataService.java:275)
                    at org.elasticsearch.search.aggregations.support.ValuesSourceParser.config(ValuesSourceParser.java:209)
                    at org.elasticsearch.search.aggregations.bucket.terms.TermsParser.parse(TermsParser.java:76)
                    at org.elasticsearch.search.aggregations.AggregatorParsers.parseAggregators(AggregatorParsers.java:198)
                    at org.elasticsearch.search.aggregations.AggregatorParsers.parseAggregators(AggregatorParsers.java:103)
                    at org.elasticsearch.search.aggregations.AggregationParseElement.parse(AggregationParseElement.java:60)
                    at org.elasticsearch.search.SearchService.parseSource(SearchService.java:838)
                    ... 12 more

    1. fixed by [using *.raw](https://github.com/elastic/elasticsearch/issues/15267)

1. failed parsing date from field

    1. run logstash -f goog.conf

            Failed parsing date from field {:field=>"date_of_record", :value=>"date_of_record", :exception=>"Invalid format: \"date_of_record\"", :config_parsers=>"yyyy-MM-dd", :config_locale=>"default=en_US", :level=>:warn}

    1. fixed

            remove first line from goog.csv
            i.e. remove column names