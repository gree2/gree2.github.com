---
layout: post
title: "command ln difference between soft link and hard link"
description: ""
category: [command]
tags: [ln, soft link, hard link]
---
{% include JB/setup %}

### [info](http://www.computerhope.com/unix/uln.htm)

1. `ln` creates links between files

1. ln option target linkname

    * `ln` creates a link to file `target`

    * with the name `linkname`

    * `ln` creates `hard links` by default

    * `-s` or `--symbolic` for symbolic links

### [demo](https://www.moreofless.co.uk/linux-difference-soft-symbolic-link-and-hard-link/)

1. create file and link

            # create a file `a`
            $ echo "hello" > a

            # create `hard link`
            # called `b` to `a`
            $ ln a b

            # create `soft link`
            # called `c` to `a`
            $ ln -s a c

1. printing the contents

            $ cat a
            hello

            $ cat b
            hello

            $ cat c
            hello

1. remove file `a`

            $ rm a

1. printing the contents again

            $ cat a
            cat: a: No such file or directory
            
            $ cat b
            a
            
            $ cat c
            cat: c: No such file or directory