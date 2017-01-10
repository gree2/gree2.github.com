---
layout: post
title: "mq introduction to zeromq"
description: ""
category: [mq]
tags: [mq, zeromq, activemq, webspheremq, rabbitmq, topology]
---
{% include JB/setup %}


### intro

1. [zeromq.org](http://zeromq.org/)

1. distributed messaging

    1. any language on any platform

    1. pub-sub, push-pull, router-dealer `pattern`

    1. high-speed aio engines in a tiny `library`

    1. any `architecture` centralized, distributed, small, large

    1. `brokerless` does `not store` messages on disk

### internal messaging within storm worker processes

1. from [understanding storm internal message buffers](http://www.michael-noll.com/blog/2013/06/21/understanding-storm-internal-message-buffers/)

    1. intra-worker (inter-thread on same storm node) `lmax disruptor`

    1. inter-worker (node-to-node across the network) `zeromq` or `netty`

    1. inter-topology nothing built into storm

            # you must take care of this yourself
            # e.g. kafka/rabbitmq/database etc.

1. illustration

            +---------------------------------------------------------------------+
            |                             worker process                          |
            |                +-----------------------------------+                |
            |                |          executor m of n          |                |
            |                |   +---------+       +---------+   |                |
            |                |   | user    |       |         |   |                |
            | +------------+ |   | logic   |→→→    | send    |→→→→ +------------+ |
            | |  worker    | |   | thread  |   ↓   | thread  |   |↓|  worker    | |
            | |  receive   | |   +---------+   ↓   +---------+   |↓|  send      | |
            | |  thread    | |        ↑        ↓        ↑        |↓|  thread    | |
            | | __________ | |   ___________   ↓   ___________   |↓| __________ | |
            | |  RX QUEUE →→→→→→  INC QUEUE     →→  OUT QUEUE    | →  TX QUEUE  | |
            | | __________ |↓|   ___________       ___________   |↑| __________ | |
            | +------↑-----+↓+-----------------------------------+↑+------↓-----+ |
            |        ↑       →→→ to other executors from other →→→        ↓       |
            +--------↑----------------------------------------------------↓-------+
                     ↑ topology.receiver.buffer.size(8)                   ↓        
                     ↑                topology.transfer.buffer.size(1024) ↓        
            ---------□--------------------network-layer-------------------□--------
            incoming tcp port(6700/tcp)                   outgoing tcp port(random)

    1. incoming tcp port(6700/tcp)

            one of the ports defined by `supervisor.slots.ports`

    1. outgoing tcp port(random)

    1. worker receive thread

            each worker has a single receive thread
            that listens on the `worker port`
            it puts incoming msgs from the network
            on the executors' `incoming queues`

    1. inc queue

            each element of the disruptor receive queue is a list of tuples
            here tuples are appended in batch

            topology.executor.receive.buffer.size(1024)

    1. send thread

            takes msgs from its `outgoing queue`
            and puts them on the shared `transfer queue`

    1. out queue

            each element of this disruptor `send queue` contains a `single` tuple

            topology.executor.send.buffer.size(1024)

    1. worker send thread

            reads from the disruptor `transfer queue`
            to send tuples over the network
            queue is a list of tuple
