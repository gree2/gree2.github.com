---
layout: post
title: "apache oozie workflow scheduler for hadoop"
description: ""
category: [bigdata]
tags: [oozie, workflow, scheduler, hadoop]
---
{% include JB/setup %}

### overview

1. oozie is a workflow scheduler system to manage apache hadoop jobs

1. oozie workflow jobs are directed acyclical graphs (dags) of actions

1. oozie coordinator jobs are recurrent oozie workflow jobs triggered by time (frequency) and data availablity

1. oozie is integrated with the rest of hadoop stack

    1. hadoop jobs

            * java map-reduce
            * streaming map-reduce
            * pig
            * hive
            * sqoop
            * dictcp

    1. system specific jobs

            * java programs
            * shell scripts

### [site](http://oozie.apache.org/)