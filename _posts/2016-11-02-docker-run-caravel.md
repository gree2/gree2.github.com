---
layout: post
title: "docker run caravel"
description: ""
category: [docker, viz]
tags: [caravel, python3, mysql, postgresql, sqlite]
---
{% include JB/setup %}


### setup

1. [run demo](https://hub.docker.com/r/amancevice/caravel/)

    1. steps

            $ docker pull amancevice/caravel
            $ docker run --name caravel -d -p 8088:8088 amancevice/caravel
            $ docker exec -it caravel demo

    1. visit [http://localhost:8088](http://localhost:8088) to see the demo

