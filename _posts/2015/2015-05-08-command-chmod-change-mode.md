---
layout: post
title: "command chmod change mode"
description: ""
category: [command]
tags: [chmod, file permission, file mode]
---
{% include JB/setup %}


### about

`chmod` is used to change the `permissions` of `files` or `directories`

### usage

chmod options permissions filename

            4 stands for `read`
            2 stands for `write`
            1 stands for `execute`
            0 stands for `no permission`

* chmod 644 filename

    1. owner can read and write

    1. group can read only

    1. others can read only

* read and write by everyone

            $ chmod 666 filename
            $ chmod a=rw filename