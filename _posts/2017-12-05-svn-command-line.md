---
layout: post
title: "svn command line"
description: ""
category: [command]
tags: [svn, import, commit, ci, checkout, co, log, diff]
---
{% include JB/setup %}


### svn

1. import code to repo

    $ svn import --username gree2 --password gree2 -m "initial message" https://192.168.100.110:8443/svn/demo

1. check out a repo

    $ svn co --username gree2 --password gree2 https://192.168.100.110:8443/svn/demo demo
    $ svn checkout --username gree2 --password gree2 https://192.168.100.110:8443/svn/demo demo

1. commit a change

    $ svn ci -m "change message"
    $ svn commit -m "change message"

1. [changes made to a file](https://stackoverflow.com/questions/282802/how-can-i-view-all-historical-changes-to-a-file-in-svn/24938573#24938573)

    $ svn log --diff demo.py
