---
layout: post
title: "run cypher query from sublime text 3 against neo4j"
description: ""
category: [docker, database]
tags: [cypher, st3, neo4j]
---
{% include JB/setup %}


### steps

1. [https://github.com/sqlcook/Sublime-Neo4j](https://github.com/sqlcook/Sublime-Neo4j)

1. installation

    1. clone this repo to `~/Library/Application Support/Sublime Text 3/Packages`

            $ cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages
            $ git clone https://github.com/sqlcook/Sublime-Neo4j.git

1. configuration

    1. Preferences -> Package Settings -> Neo4j Menu

            {
                "neo4j_api" : "http://localhost:7474/db/data/cypher",
                "neo4j_user" : "neo4j",
                "neo4j_password" : "node"
            }

1. keymap

    1. results are displayed in the console

            ctrl+~

    1. run query with

            ctrl+r
