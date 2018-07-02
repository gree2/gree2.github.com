---
layout: post
title: "command screen"
description: ""
category: [command]
tags: [screen, kill, session, detach, attach, share]
---
{% include JB/setup %}


### info

1. screen manager with vt100/ansi terminal emulation

### demo

1. create

    1. create with name

            $ ssh root@demo
            $ screen -S gree2
            $ # do sth
            $ ls
            ...
            $ mkdir demo
            ...
            $ ls
            ...

    1. detach running session

            $ ^a d

1. list all sesstions and reattach

    1. list

            $ screen -ls
            There are screens on:
                13494.gree2 (Detached)
                11576.pts-0.node4   (Detached)
            2 Sockets in /var/run/screen/S-root.

    1. reattach

            $ screen -r gree2
            $ # or
            $ screen -r 13494

            $ # do sth
            $ ls
            ...
            $ mkdir demo
            ...
            $ ls
            ...

1. share session multi display mode

    1. attach to a not detached screen session

            $ # on a different host
            $ ssh root@demo
            $ screen -ls
            There are screens on:
                13494.gree2 (Attached)
                11576.pts-0.node4   (Detached)
            2 Sockets in /var/run/screen/S-root.

            $ screen -r gree2
            $ # do sth
            $ ls
            ...
            $ mkdir demo
            ...
            $ ls
            ...

1. destroy current window

    1. use ^a k

            $ ^a k

            Really kill this window [y/n]
            [screen is terminating]

            $ screen -ls
            There is a screen on:
                11576.pts-0.node4   (Detached)
            1 Socket in /var/run/screen/S-root.

1. download docker image from screen session

    1. start a new sesstion

            $ screen -S mssql
            $ docker pull microsoft/mssql-server-linux:2017-CU8
            2017-CU8: Pulling from microsoft/mssql-server-linux
            f6fa9a861b90: Pull complete
            da7318603015: Pull complete
            6a8bd10c9278: Pull complete
            ...

            $ ^a d

    1. wait

    1. reattach mssql session

            $ screen -r mssql
            docker pull microsoft/mssql-server-linux:2017-CU8
            2017-CU8: Pulling from microsoft/mssql-server-linux
            f6fa9a861b90: Pull complete
            da7318603015: Pull complete
            6a8bd10c9278: Pull complete
            d5a40291440f: Pull complete
            bbdd8a83c0f1: Pull complete
            3a52205d40a6: Pull complete
            6192691706e8: Pull complete
            1a658a9035fb: Pull complete
            655776156720: Pull complete
            0c695ca332a2: Pull complete
            Digest: sha256:70e2a80e516935701ca109c57fa2aede152460d2da6f20d349afce8a93156a28
            Status: Downloaded newer image for microsoft/mssql-server-linux:2017-CU8

            $ docker images
            REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
            microsoft/mssql-server-linux    2017-CU8            229d30f7b467        2 weeks ago         1.43 GB

    1. kill mssql session

            $ ^a k
            [screen is terminating]
            $ screen -ls
            No Sockets found in /var/run/screen/S-root.

### references

1. [https://www.computerhope.com/unix/screen.htm](https://www.computerhope.com/unix/screen.htm)
