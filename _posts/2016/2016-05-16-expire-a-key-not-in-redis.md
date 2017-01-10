---
layout: post
title: "expire a key not in redis"
description: ""
category: [python]
tags: [redis, expire]
---
{% include JB/setup %}


### poc

1. [if you expire a key not in redis](https://github.com/gree2/hobby/blob/master/python/p.redis/expire.poc.py)

    1. not exist

            # False
            print conn.expire('not-exist-in-redis', 10)

    1. exist

            # True
            print conn.set('exist-in-redis', 'yeah')
            # True
            print conn.expire('exist-in-redis', 0)
            # None
            print conn.get('exist-in-redis')
