---
layout: post
title: "elk stack kibana"
description: ""
category: [bigdata]
tags: [elk, elasticsearch, logstash, kibana]
---
{% include JB/setup %}


### kibana

1. [kibana](https://www.elastic.co/products/kibana)

1. [downloads](https://www.elastic.co/downloads/kibana)

1. documentation

	1. [4.5(current)](https://www.elastic.co/guide/en/kibana/current/index.html)

	1. [5.0](https://www.elastic.co/guide/en/kibana/5.0/index.html)

1. install steps

	1. downloads

			# linux 64-bit
			$ curl -O https://download.elastic.co/kibana/kibana/kibana-4.5.1-linux-x64.tar.gz
			# deb 64-bit
			$ curl -O https://download.elastic.co/kibana/kibana/kibana_4.5.1_amd64.deb

			### note kibana 4.5.x requires elasticsearch 2.3.x

	1. config and run

			# 1. config `config/kibana.yml`
			$ pico kibana.yml
			elasticsearch.url: http://localhost:9200

			# 2. run
			# 2.1 on linux
			$ bin/kibana

			# 2.2 on windows
			> bin/kibana.bat

	1. view

			# browser
			http://localhost:5601


### docker-kibana

1. [git repo](https://github.com/gree2/hello-docker/tree/master/docker-elk/docker-kibana)