---
layout: post
title: "redis advanced data types"
description: ""
category: [nosql]
tags: [redis, data type]
---
{% include JB/setup %}


### sets

1. redis-cli

    1. sadd

            redis 127.0.0.1:6379> sadd user:max:favorite "arcade fire" "arctic monkeys" "belle & sebastian" "lenine
            (integer) 4
            redis 127.0.0.1:6379> sadd user:hugo:favorite "daft punk" "the kooks" "arctic monkeys"
            (integer) 3

    1. sinter

            redis 127.0.0.1:6379> sinter user:max:favorite user:hugo:favorite
            1) "arctic monkeys"

    1. sdiff

            redis 127.0.0.1:6379> sdiff user:max:favorite user:hugo:favorite
            1) "lenine"
            2) "belle & sebastian"
            3) "arcade fire"

            redis 127.0.0.1:6379> sdiff user:hugo:favorite user:max:favorite
            1) "daft punk"
            2) "the kooks"

    1. sunion

            redis 127.0.0.1:6379> sunion user:max:favorite user:hugo:favorite
            1) "belle & sebastian"
            2) "arcade fire"
            3) "arctic monkeys"
            4) "daft punk"
            5) "the kooks"
            6) "lenine"

    1. srandmember

            redis 127.0.0.1:6379> srandmember user:max:favorite
            "arcade fire"
            redis 127.0.0.1:6379> srandmember user:max:favorite
            "belle & sebastian"

    1. sismember

            redis 127.0.0.1:6379> sismember user:max:favorite "arctic monkeys"
            (integer) 1

    1. srem

            redis 127.0.0.1:6379> srem user:max:favorite "arctic monkeys"
            (integer) 1

    1. scard

            redis 127.0.0.1:6379> scard user:max:favorite
            (integer) 3
            redis 127.0.0.1:6379> scard user:hugo:favorite
            (integer) 3

    1. smembers
    
            redis 127.0.0.1:6379> smembers user:max:favorite
            1) "lenine"
            2) "belle & sebastian"
            3) "arcade fire"
            redis 127.0.0.1:6379> smembers user:hugo:favorite
            1) "arctic monkeys"
            2) "daft punk"
            3) "the kooks"

### sorted sets

### bitmaps

### hyperloglogs