---
layout: post
title: "command diff"
description: ""
category: [command]
tags: [diff, st3, clipboard.diff]
---
{% include JB/setup %}


### demo

1. [diff to output only the file names](https://stackoverflow.com/questions/6217628/diff-to-output-only-the-file-names)

    1. compare 2 folder

            $ diff -rq elasticsearch/elasticsearch-2.0.0-transwarp/lib/ elasticsearch4.6/elasticsearch-2.0.0-transwarp/lib/
            Only in elasticsearch/elasticsearch-2.0.0-transwarp/lib/: dnw.jar
            Only in elasticsearch4.6/elasticsearch-2.0.0-transwarp/lib/: elasticsearch-2.0.0-transwarp-tdh460.jar
            Only in elasticsearch/elasticsearch-2.0.0-transwarp/lib/: elasticsearch-2.0.0-transwarp-tdh482.jar
            Only in elasticsearch/elasticsearch-2.0.0-transwarp/lib/: hamcrest-core-1.3.jar
            Only in elasticsearch/elasticsearch-2.0.0-transwarp/lib/: htrace-core-3.1.0-incubating.jar
            Only in elasticsearch/elasticsearch-2.0.0-transwarp/lib/: junit-4.11.jar

    1. use sublime text plugin `clipboard diff`

            # step.1
            $ ls elasticsearch/elasticsearch-2.0.0-transwarp/lib/
            ...

            # step.2
            # copy and paste output to st

            # step.3
            $ ls elasticsearch4.6/elasticsearch-2.0.0-transwarp/lib/
            ...

            # step.4
            # copy

            # step.5
            # select text in step 2 and press ctrl+alt+cmd+d
