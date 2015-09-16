---
layout: post
title: "redis on mac"
description: ""
category: [nosql]
tags: [redis]
---
{% include JB/setup %}


### on mac

1. install

            $ brew install redis

1. start `redis-server`

            $ redis-server

1. start `redis-cli`

            $ redis-cli

1. demo

    1. simple

            > set gree2 hqlgree2
            OK
            > get gree2
            "hqlgree2"
            > keys g*
            1) "gree2"

    1. `mset`

            > mset first "value 1" second "value 2"
            OK
            > mget first second
            1) "value 1"
            2) "value 2"

    1. ttl

            # >0 seconds
            # -2 key is expired or does not exist
            # -1 exist but no expiration time set

            > set gree2 hqlgree2
            > set gree2 hqlgree2
            OK
            > expire gree2 10
            (integer) 1
            > ttl gree2
            (integer) 4
            > ttl gree2
            (integer) 0
            > ttl gree2
            (integer) -1
            > ttl gree2
            (integer) -1
            > get gree2
            (nil)

    1. incr

            > set counter 100
            OK
            > incr counter
            (integer) 101
            > incrby counter 5
            (integer) 106
            > incr counter
            (integer) 107
            > decr counter
            (integer) 106
            > decrby counter 100
            (integer) 6
            > get counter
            "6"
            > incrbyfloat counter 2.4
            "8.4"

### node.js demo

1. install `redis` for node.js

            $ npm install redis
            redis@1.0.0 node_modules/redis

1. demo 1

    1. touch `hello.js`

            $ pico hello.js
            var redis = require("redis");
            var client = redis.createClient();
            client.set("gree2", "hqlgree2 from node.js");
            client.get("gree2", redis.print);
            client.quit();

    1. run `hello.js`

            $ node hello.js
            "hqlgree2 from node.js"

1. demo 2

    1. touch `vote.js`

            $ pico vote.js
            var redis = require("redis");
            var client = redis.createClient();
            client.set("article:100:headline", "hqlgree2's book one");
            client.set("article:135:headline", "hqlgree2's book two");

            function upVote(id){
                var key = "article:" + id + ":votes";
                client.incr(key);
            }

            function downVote(id){
                var key = "article:" + id + ":votes";
                client.decr(key);
            }

            function showResults(id){
                var headlineKey = "article:" + id + ":headline";
                var voteKey = "article:" + id + ":votes";
                client.mget([headlineKey, voteKey], function(err, replies){
                    console.log('the article "' + replies[0] + '" has', replies[1], 'votes');
                });
            }

            upVote(100);
            upVote(100);
            upVote(100);
            upVote(135);
            upVote(135);
            downVote(135);

            showResults(100);
            showResults(135);

            client.quit();

    1. run

            $ node vote.js
            the article "hqlgree2's book one" has 9 votes
            the article "hqlgree2's book two" has 3 votes

