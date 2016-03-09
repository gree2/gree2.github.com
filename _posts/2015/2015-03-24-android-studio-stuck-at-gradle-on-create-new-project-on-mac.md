---
layout: post
title: "android studio stuck at gradle on create new project on mac"
description: ""
category: [android]
tags: [mac, create, project, gradle]
---
{% include JB/setup %}

### solution

1. shut down android studio (may have to force kill)

1. go to your `~/.gradle` form terminal

1. under `wrapper/dists` dir there is a whatever version of gradle

1. `cd gradle-2.2.1-all` you will see a folder like this `c64ydeuardnfqctvr1gm30w53`

            $ cd c64ydeuardnfqctvr1gm30w53
            $ ls 
            gradle-2.2.1-all.zip.lck
            gradle-2.2.1-all.zip.part

1. delete these files and download `gradle-2.2.1-all.zip`

1. put `gradle-2.2.1-all.zip` in `c64ydeuardnfqctvr1gm30w53` foder

1. launch android studio and `cd` to this folder

            $ cd ~/.gradle/wrapper/dists/gradle-2.2.1-all/c64ydeuardnfqctvr1gm30w53
            $ ls
            total 114464
            drwxr-xr-x  13 hqlgree2  staff       442 Mar 24 21:22 gradle-2.2.1
            -rw-r-----@  1 hqlgree2  staff  58602849 Mar 24 21:20 gradle-2.2.1-all.zip
            -rw-r--r--   1 hqlgree2  staff         0 Mar 24 21:22 gradle-2.2.1-all.zip.lck
            -rw-r--r--   1 hqlgree2  staff         0 Mar 24 21:22 gradle-2.2.1-all.zip.ok
