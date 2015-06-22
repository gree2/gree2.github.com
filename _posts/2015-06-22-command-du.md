---
layout: post
title: "command du"
description: ""
category: [command]
tags: [du, disk space]
---
{% include JB/setup %}


### info

1. `du` estimates and displays the `disk space` used by files

1. size format

    1. size is an integer and optional unit

    1. units are `K`, `M`, `G`, `T`, `P`, `E`, `Z`, `Y`

### demo

1. [computerhope demo](http://www.computerhope.com/unix/udu.htm)

    1. report the size of each file in current directory with the `extension .txt`

            $ du -s *.txt

    1. bu in a `human-readable` size format

            $ du -shc *.txt

1. [cyberciti demo](http://www.cyberciti.biz/faq/checking-directory-size-in-linux-command/)

    1. without any options shows the `names` and `used space` for each directories including all sub-directories in the `current directory`

            $ du
            64      ./.ipynb_checkpoints
            1395520 ./backup
            64      ./infocatalog/ETLService
            25192   ./infocatalog
            1417488 ./input
            364752  ./output
            288     ./sql
            3203424 .

    1. pass `-s` to see `total` disk space used by a directory

            $ du -sh
            1.5G    .

            $ du -sh backup/ input/ output/
            681M    backup/
            692M    input/
            178M    output/

    1. pass `-c` to see a `grand total` for all of the files

            $ du -csh backup/ input/ output/
            681M    backup/
            692M    input/
            178M    output/
            1.5G    total