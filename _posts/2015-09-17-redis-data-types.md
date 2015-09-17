---
layout: post
title: "redis data types"
description: ""
category: [nosql]
tags: [redis, data type]
---
{% include JB/setup %}


### strings

1. redis-cli

    1. mset

            redis 127.0.0.1:6379> mset first "key value 1" second "key value 2"
            OK

    1. mget

            redis 127.0.0.1:6379> mget first second
            1) "key value 1"
            2) "key value 2"

    1. expire

            redis 127.0.0.1:6379> set timetolive "10"
            OK
            redis 127.0.0.1:6379> expire timetolive 10
            (integer) 1
            redis 127.0.0.1:6379> get timetolive
            "10"
            redis 127.0.0.1:6379> tll timetolive
            (integer) 5
            redis 127.0.0.1:6379> tll timetolive
            (integer) 2
            redis 127.0.0.1:6379> tll timetolive
            (error) ERR unknown command 'tll'
            redis 127.0.0.1:6379> get timetolive
            (nil)

    1. incr incrby decr decrby incrbyfloat

            redis 127.0.0.1:6379> set counter 100
            OK
            redis 127.0.0.1:6379> incr counter
            (integer) 101
            redis 127.0.0.1:6379> incrby counter 5
            (integer) 106
            redis 127.0.0.1:6379> decr counter
            (integer) 105
            redis 127.0.0.1:6379> decrby counter 100
            (integer) 5
            redis 127.0.0.1:6379> get counter
            "5"
            redis 127.0.0.1:6379> incrbyfloat counter 2.4
            "7.4"


### lists

1. redis-cli

    1. lpush

            redis 127.0.0.1:6379> lpush books "clean code"
            (integer) 1
            redis 127.0.0.1:6379> lpush books "code complete"
            (integer) 2
            redis 127.0.0.1:6379> lpush books "peopleware"
            (integer) 3

    1. llen

            redis 127.0.0.1:6379> llen books
            (integer) 3
            redis 127.0.0.1:6379> lindex books 1
            "code complete"

    1. lrange

            redis 127.0.0.1:6379> lrange books 0 1
            1) "peopleware"
            2) "code complete"
            redis 127.0.0.1:6379> lrange books 0 -1
            1) "peopleware"
            2) "code complete"
            3) "clean code"

    1. lpop rpop

            redis 127.0.0.1:6379> lpop books
            "peopleware"
            redis 127.0.0.1:6379> rpop books
            "clean code"
            redis 127.0.0.1:6379> lrange books 0 -1
            1) "code complete"

### hashes

1. redis-cli

    1. hset

            redis 127.0.0.1:6379> hset movie "title" "the godfather"
            (integer) 1

    1. hmset
    
            redis 127.0.0.1:6379> hmset movie "year" 1972 "rating" 9.2 "watches" 10000000
            OK

    1. hincrby
    
            redis 127.0.0.1:6379> hincrby movie "watches" 3
            (integer) 10000003
    
    1. hget

            redis 127.0.0.1:6379> hget movie "title"
            "the godfather"

    1. hmget

            redis 127.0.0.1:6379> hmget movie "title" "watches"
            1) "the godfather"
            2) "10000003"
    
    1. hdel

            redis 127.0.0.1:6379> hdel movie "watches"
            (integer) 1

    1. hgetall

            redis 127.0.0.1:6379> hgetall movie
            1) "title"
            2) "the godfather"
            3) "year"
            4) "1972"
            5) "rating"
            6) "9.2"

    1. hkeys

            redis 127.0.0.1:6379> hkeys movie
            1) "title"
            2) "year"
            3) "rating"

    1. hvals

            redis 127.0.0.1:6379> hvals movie
            1) "the godfather"
            2) "1972"
            3) "9.2"
