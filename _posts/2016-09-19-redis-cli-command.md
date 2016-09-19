---
layout: post
title: "redis cli command"
description: ""
category: [command]
tags: [redis-cli, command]
---
{% include JB/setup %}


### commands

1. all keys

    1. list all keys

            $ redis-cli keys *

    1. delete all keys with a special prefix

            $ redis-cli keys "key_prefix*" | xargs redis-cli del

