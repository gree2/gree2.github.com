---
layout: post
title: "rabbitmq docker"
description: ""
category: [docker, python]
tags: [rabbitmq, docker, pika, python, producer, consumer]
---
{% include JB/setup %}


### docker

1. steps

    1. pull image from docker hub

            $ docker pull rabbitmq

    1. running the daemon

            $ docker run -d --hostname grabbit --name grabbit rabbitmq

    1. check logs

            $ docker logs grabbit

                          RabbitMQ 3.6.2. Copyright (C) 2007-2016 Pivotal Software, Inc.
              ##  ##      Licensed under the MPL.  See http://www.rabbitmq.com/
              ##  ##
              ##########  Logs: tty
              ######  ##        tty
              ##########
                          Starting broker...

            =INFO REPORT==== 6-Jun-2016::03:49:08 ===
            Starting RabbitMQ 3.6.2 on Erlang 18.3
            Copyright (C) 2007-2016 Pivotal Software, Inc.
            Licensed under the MPL.  See http://www.rabbitmq.com/

            =INFO REPORT==== 6-Jun-2016::03:49:08 ===
            node           : rabbit@grabbit
            home dir       : /var/lib/rabbitmq
            config file(s) : /etc/rabbitmq/rabbitmq.config
            cookie hash    : OmX9qesYyfQMInMBuF76rg==
            log            : tty
            sasl log       : tty
            database dir   : /var/lib/rabbitmq/mnesia/rabbit@grabbit

            =INFO REPORT==== 6-Jun-2016::03:49:08 ===
            Memory limit set to 398MB of 995MB total.

            =INFO REPORT==== 6-Jun-2016::03:49:08 ===
            Disk free limit set to 50MB

            =INFO REPORT==== 6-Jun-2016::03:49:08 ===
            Limiting to approx 1048476 file handles (943626 sockets)

            =INFO REPORT==== 6-Jun-2016::03:49:08 ===
            FHC read buffering:  OFF
            FHC write buffering: ON

            =INFO REPORT==== 6-Jun-2016::03:49:09 ===
            Database directory at /var/lib/rabbitmq/mnesia/rabbit@grabbit is empty. Initialising from scratch...

            =INFO REPORT==== 6-Jun-2016::03:49:09 ===
                application: mnesia
                exited: stopped
                type: temporary

            =INFO REPORT==== 6-Jun-2016::03:49:09 ===
            Priority queues enabled, real BQ is rabbit_variable_queue

            =INFO REPORT==== 6-Jun-2016::03:49:09 ===
            Adding vhost '/'

            =INFO REPORT==== 6-Jun-2016::03:49:09 ===
            Creating user 'guest'

            =INFO REPORT==== 6-Jun-2016::03:49:09 ===
            Setting user tags for user 'guest' to [administrator]

            =INFO REPORT==== 6-Jun-2016::03:49:09 ===
            Setting permissions for 'guest' in '/' to '.*', '.*', '.*'

            =INFO REPORT==== 6-Jun-2016::03:49:09 ===
            msg_store_transient: using rabbit_msg_store_ets_index to provide index

            =INFO REPORT==== 6-Jun-2016::03:49:09 ===
            msg_store_persistent: using rabbit_msg_store_ets_index to provide index

            =WARNING REPORT==== 6-Jun-2016::03:49:09 ===
            msg_store_persistent: rebuilding indices from scratch

            =INFO REPORT==== 6-Jun-2016::03:49:09 ===
            started TCP Listener on [::]:5672
             completed with 0 plugins.

            =INFO REPORT==== 6-Jun-2016::03:49:09 ===
            Server startup complete; 0 plugins started.

1. hello world

    1. install python `pika` package

            $ pip install pika

    1. [hello.world.producer.py](https://github.com/gree2/hobby/blob/master/python/p.rabbitmq/hello.world.producer.py)

    1. [hello.world.consumer.py](https://github.com/gree2/hobby/blob/master/python/p.rabbitmq/hello.world.consumer.py)

    1. test

            run consumer first
            then run producer

### fixed

1. raise exceptions.ConnectionClosed() pika.exceptions.ConnectionClosed

    1. fixed

            $ docker stop grabbit
            $ docker rm grabbit
            $ docker run -d --hostname grabbit --name grabbit -p 4369:4369 -p 5671:5671 -p 5672:5672 -p 25762:25762 rabbitmq
