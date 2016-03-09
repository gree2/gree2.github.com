---
layout: post
title: "storm parallelism"
description: ""
category: [realtime]
tags: [node, worker, executor, task, parallelism]
---
{% include JB/setup %}


### main components

1. nodes - machines

1. workers - jvms

1. executors - threads

1. tasks - bolt/spout instance

### topology execution

1. config workers

    1. `1` sentence spout `1` split bolt `1` count bolt `1` report bolt

            Config config = new Config();
            config.setNumWorkers(2);
            builder.setSpout(SENTENCE_SPOUT_ID, spout);
            builder.setBolt(SPLIT_BOLT_ID, splitBolt);
            builder.setBolt(COUNT_BOLT_ID, countBolt)
                .fieldGrouping(SPLIT_BOLT_ID, new Fields("word");
            builder.setBolt(REPORT_BOLT_ID, reportBolt)
                .globalGrouping(COUNT_BOLT_ID);

    1. using `1` worker

            +-----------------------------------------------------------------+
            |                              node                               |
            | +-------------------------------------------------------------+ |
            | |                          worker (JVM)                       | |
            | | +------------+ +------------+ +------------+ +------------+ | |
            | | |  executor  | |  executor  | |  executor  | |  executor  | | |
            | | |  (thread)  | |  (thread)  | |  (thread)  | |  (thread)  | | |
            | | | +--------+ | |  --------  | |  --------  | |  --------  | | |
            | | | |  task  | | | /  task  \ | | /  task  \ | | /  task  \ | | |
            | | | |  spout | | | |  bolt  | | | |  bolt  | | | |  bolt  | | | |
            | | | |        | | | |        | | | |        | | | |        | | | |
            | | | |sentence| | | |sentence| | | |  word  | | | | report | | | |
            | | | |        | | | \  split / | | \  count / | | \        / | | |
            | | | +--------+ | |  --------  | |  --------  | |  --------  | | |
            | | +------------+ +------------+ +------------+ +------------+ | |
            | +-------------------------------------------------------------+ |
            +-----------------------------------------------------------------+

### adding workers to a topology

1. config workers

    1. code

            Config config = new Config();
            config.setNumWorkers(2);

1. config executors and tasks

    1. code

            builder.setSpout(SENTENCE_SPOUT_ID, spout, 2);

    1. using `one worker`

            +-----------------------------------------------------------------+
            |                              node                               |
            | +-------------------------------------------------------------+ |
            | |                          worker (JVM)                       | |
            | | +------------+ +------------+ +------------+ +------------+ | |
            | | |  executor  | |  executor  | |  executor  | |  executor  | | |
            | | |  (thread)  | |  (thread)  | |  (thread)  | |  (thread)  | | |
            | | | +--------+ | |  --------  | |  --------  | |  --------  | | |
            | | | |  task  | | | /  task  \ | | /  task  \ | | /  task  \ | | |
            | | | |  spout | |>| |  bolt  | |>| |  bolt  | |>| |  bolt  | | | |
            | | | |        | | | |        | | | |        | | | |        | | | |
            | | | |sentence| | | |sentence| | | |  word  | | | | report | | | |
            | | | |        | | | \  split / | | \  count / | | \        / | | |
            | | | +--------+ | |  --------  | |  --------  | |  --------  | | |
            | | +------------+ +------------+ +------------+ +------------+ | |
            | | +------------+        ^                                     | |
            | | |  executor  |        |                                     | |
            | | |  (thread)  |        |                                     | |
            | | | +--------+ |        |                                     | |
            | | | |  task  | |        |                                     | |
            | | | |  spout | |--------                                      | |
            | | | |        | |                                              | |
            | | | |sentence| |                                              | |
            | | | |        | |                                              | |
            | | | +--------+ |                                              | |
            | | +------------+                                              | |
            | +-------------------------------------------------------------+ |
            +-----------------------------------------------------------------+

1. setup split sentence bolt and word count bolt

    1. `split sentence bolt` execute as `4 tasks` with `2 executors`

            builder.setBolt(SPLIT_BOLT_ID, splitBolt, 2)
                .setNumTask(4)
                .shuffleGrouping(SENTENCE_SPOUT_ID);

    1. `word count bolt` execute as `4 tasks` each with its own executor thread

            builder.setBolt(COUNT_BOLT_ID, countBolt, 4)
                .fieldGrouping(SPLIT_BOLT_ID, new Fields("word"));

    1. using `2 workers`

            +-----------------------------------------------------------------+
            |                              node                               |
            | +-------------------------------------------------------------+ |
            | |                          worker (JVM)                       | |
            | | +------------+ +------------+ +------------+ +------------+ | |
            | | |  executor  | |  executor  | |  executor  | |  executor  | | |
            | | |  (thread)  | |  (thread)  | |  (thread)  | |  (thread)  | | |
            | | | +--------+ | |  --------  | |  --------  | |  --------  | | |
            | | | |  task  | | | /  task  \ | | /  task  \ | | /  task  \ | | |
            | | | |  spout | | | |  bolt  | | | |  bolt  | | | |  bolt  | | | |
            | | | |        | | | |        | | | |        | | | |        | | | |
            | | | |sentence| | | |sentence| | | |  word  | | | | report | | | |
            | | | |        | | | \  split / | | \  count / | | \        / | | |
            | | | +--------+ | |  --------  | |  --------  | |  --------  | | |
            | | +------------+ |  --------  | +------------+ +------------+ | |
            | |                | /  task  \ | +------------+                | |
            | |                | |  bolt  | | |  executor  |                | |
            | |                | |        | | |  (thread)  |                | |
            | |                | |sentence| | |  --------  |                | |
            | |                | \  split / | | /  task  \ |                | |
            | |                |  --------  | | |  bolt  | |                | |
            | |                +------------+ | |        | |                | |
            | |                               | |  word  | |                | |
            | |                               | \  count / |                | |
            | |                               |  --------  |                | |
            | |                               +------------+                | |
            | +-------------------------------------------------------------+ |
            | +-------------------------------------------------------------+ |
            | |                          worker (JVM)                       | |
            | | +------------+ +------------+ +------------+ +------------+ | |
            | | |  executor  | |  executor  | |  executor  | |  executor  | | |
            | | |  (thread)  | |  (thread)  | |  (thread)  | |  (thread)  | | |
            | | | +--------+ | |  --------  | |  --------  | |  --------  | | |
            | | | |  task  | | | /  task  \ | | /  task  \ | | /  task  \ | | |
            | | | |  spout | | | |  bolt  | | | |  bolt  | | | |  bolt  | | | |
            | | | |        | | | |        | | | |        | | | |        | | | |
            | | | |sentence| | | |sentence| | | |  word  | | | | report | | | |
            | | | |        | | | \  split / | | \  count / | | \        / | | |
            | | | +--------+ | |  --------  | |  --------  | |  --------  | | |
            | | +------------+ |  --------  | +------------+ +------------+ | |
            | |                | /  task  \ | +------------+                | |
            | |                | |  bolt  | | |  executor  |                | |
            | |                | |        | | |  (thread)  |                | |
            | |                | |sentence| | |  --------  |                | |
            | |                | \  split / | | /  task  \ |                | |
            | |                |  --------  | | |  bolt  | |                | |
            | |                +------------+ | |        | |                | |
            | |                               | |  word  | |                | |
            | |                               | \  count / |                | |
            | |                               |  --------  |                | |
            | |                               +------------+                | |
            | +-------------------------------------------------------------+ |
            +-----------------------------------------------------------------+
