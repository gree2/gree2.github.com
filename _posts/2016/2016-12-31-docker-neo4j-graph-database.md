---
layout: post
title: "docker neo4j graph database"
description: ""
category: [docker, database, viz]
tags: [neo4j, graph database]
---
{% include JB/setup %}


### steps

1. [official documentation](https://neo4j.com/developer/docker/)

1. docker container up and running

    1. pull neo4j image

            $ docker pull neo4j

    1. run container

            $ docker run \
            --publish=7474:7474 --publish=7687:7687 \
            --volumn=$HOME/neo4j/data:/data \
            --volumn=$HOME/neo4j/logs:/logs \
            neo4j

    1. visit <http://localhost:7474>

            default neo4j requires authentication
            username/password
            neo4j/neo4j

    1. run container without auth

            $ docker run \
            --env=NEO4J_AUTH=none \
            --publish=7474:7474 --publish=7687:7687 \
            --volumn=$HOME/neo4j/data:/data \
            --volumn=$HOME/neo4j/logs:/logs \
            neo4j

### neo4j 3.0 docker image

1. ports exposed

        7474 for http
        7473 for https
        7687 for bolt

1. volumes exposed

        /data for persist data outside container
        /logs for access neo4j log files
