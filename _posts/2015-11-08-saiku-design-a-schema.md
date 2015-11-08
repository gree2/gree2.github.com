---
layout: post
title: "saiku design a schema"
description: ""
category: [bi]
tags: [saiku, olap, cube, dimension, measure, hierarchi]
---
{% include JB/setup %}


### design a schema

1. start saiku ee server

    1. cd to installed path

            $ cd /Applications/Saiku Enterprise/server
            $ sh start-saiku.sh

1. login

    1. go to [http://localhost:8080/](http://localhost:8080/)

            # saiku 3.6-ee
            # use evaluation login
            # username admin
            # password admin

1. menus

    1. new query

    1. open query

    1. logout

    1. about

    1. improve this translation

    1. admin console

    1. dashboads

    1. schema designer

### schema designer

1. connection

    1. click `schema designer`

            # create a new schema or open an existing project?
            # new/open/cancel

            # 1. `open`
            # edit functionality coming soon.

            # 2. `edit`
            # schema name: o2o

    1. connection details

            # connection type: sql/mongo

            # 1. sql
            # driver:   com.mysql.jdbc.Driver
            # url:      jdbc:mysql://localhost
            # username: root
            # password: ******

            # 2. mongo
            # mongo schema: mongo-o2o.json

    1. connection details - sql

            # available databases: select
            # database schema    : select - public



    1. click `get tables`

1. design snowflake

    1. drag table to canvas

            # table edit
            # table name: user
            # select key: id

            # click `assign key`

    1. connect them

            # join configuration
            # source table:       user
            # target table:       date
            # foreign key column: day

    1. repeat

1. add cubes and dimensions

    1. dimensions - view options

            # tables:         date
            # dimension name: date
            # select key:     id
            # select type:    standard/time

    1. dimensions - attributes

            # year
            # month
            # week

    1. dimensions - hierarchies

            # 1. hierarchy
            # name:            year month
            # all member name: all months

            # 2. levels
            # level: year
            #        month

    1. cubes - view options

            # cube name: user

    1. cubes - measure group

            # measure group name: user_measures
            # select table:       user

    1. cubes - add measures

            # measure name:     platform
            # select column:    platform
            # select aggregate: sum/count/average/max/min
            # format string:

    1. cubes - link dimensions

            # select dimension:   date
            # select foreign key: date

### data source

1. create

    1. details

            # name:    o2o
            # connection type: mondrian/xmla/mongo
                               ********
            # url:             jdbc:mysql://localhost
            # schema:          /datasources/o2o.xml
            # jdbc driver:     com.mysql.jdbc.Driver
            # username:        root
            # password:        ******

    1. foodmart demo

            # name:    foodmart
            # connection type: mondrian

            # url:             jdbc:h2:../../data/foodmart
            # schema:          /datasources/o2o.xml
            # jdbc driver:     org.h2.Driver
            # username:        sa
            # password:
