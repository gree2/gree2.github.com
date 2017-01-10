---
layout: post
title: "import movielens data into neo4j container"
description: ""
category: [database, viz]
tags: [movielens, neo4j, container, docker, cypher]
---
{% include JB/setup %}


### steps

1. sample dataset

    1. [ml-latest-small.zip](http://files.grouplens.org/datasets/movielens/ml-latest-small.zip) about 900KB

1. unzip and serve these csv files

            $ cd /ml-latest-small

    1. line of data

            $ wc -l *
            9126 links.csv
            48 load.cql.sq
            9126 movies.csv
            100005 ratings.csv
            1297 tags.csv
            119749 total

    1. serve these data

            $ python -m SimpleHTTPServer

1. [start neo4j container]({% post_url 2016-12-31-docker-neo4j-graph-database %})

            $ docker run --rm \
            --publish=7474:7474 \
            --publish=7687:7687 \
            --volume=$HOME/neo4j/data:/data \
            --volume=$HOME/neo4j/logs:/logs \
            --name gneo4j neo4j

1. import csv data

    1. create index

            CREATE CONSTRAINT ON (m:Movie) ASSERT m.id IS UNIQUE;
            CREATE CONSTRAINT ON (u:User) ASSERT u.id IS UNIQUE;
            CREATE CONSTRAINT ON (g:Genre) ASSERT g.name IS UNIQUE;

    1. import `movies.csv`

            //////////////////
            // movies.csv 9126
            // movieId,title,genres
            USING PERIODIC COMMIT LOAD CSV WITH HEADERS
            FROM "http://localhost:8000/movies.csv" AS line
            WITH line, SPLIT(line.genres, "|") AS Genres
            CREATE (m:Movie { id: TOINTEGER(line.`movieId`), title: line.`title` })
            WITH Genres
            UNWIND RANGE(0, SIZE(Genres)-1) as i
            MERGE (g:Genre {name: UPPER(Genres[i])})
            CREATE (m)-[r:GENRE {position:i+1}]->(g);

            // if you import from neo4j browser
            // Added 9145 labels, created 9251 nodes, set 18376 properties, created 106 relationships, statement completed in 8739 ms.

    1. import `tags.csv`

            ////////////////
            // tags.csv 1297
            // userId,movieId,tag,timestamp
            USING PERIODIC COMMIT LOAD CSV WITH HEADERS
            FROM "http://localhost:8000/tags.csv" AS line
            WITH line
            MATCH (m:Movie { id: TOINTEGER(line.`movieId`) })
            MERGE (u:User { id: TOINTEGER(line.`userId`) })
            CREATE (u)-[r:TAG {tag: line.`tag`}]->(m);
            
            // if you import from neo4j browser
            // Added 61 labels, created 61 nodes, set 1357 properties, created 1296 relationships, statement completed in 1789 ms.

    1. import `ratings.csv`

            /////////////////////
            // ratings.csv 100005
            // userId,movieId,rating,timestamp
            USING PERIODIC COMMIT LOAD CSV WITH HEADERS
            FROM "http://localhost:8000/ratings.csv" AS line
            WITH line
            MATCH (m:Movie { id: TOINTEGER(line.`movieId`) })
            MATCH (u:User { id: TOINTEGER(line.`userId`) })
            CREATE (u)-[r:RATING {rating: TOFLOAT(line.`rating`)}]->(m);

            // if you import from neo4j browser
            // Set 19072 properties, created 19072 relationships, statement completed in 11461 ms.

1. [run cypher query against this dataset]({% post_url 2016-12-31-run-cypher-query-from-sublime-text-3-against-neo4j %})

    1. find user 23

            // find user 23
            MATCH (u:User {id: 23})
            RETURN u

    1. find user 23's rating movies

            // find user 23's rating movies
            MATCH (u:User {id: 23})
            MATCH (u)-[:RATING]->(m:Movie)
            RETURN u, m
            LIMIT 10

    1. find user 23's rating movies and movies's genre

            // find user 23's rating movies and movies' genre
            MATCH (u:User {id: 23})
            MATCH (u)-[:RATING]->(m:Movie)
            MATCH (m)-[r:CATEGORY]->(g)
            RETURN u, m, g
            LIMIT 10
