---
layout: post
title: "python fabric getting started"
description: ""
category: [python]
tags: [fabric, pip, conda, apt-get]
---
{% include JB/setup %}


### [info](http://www.fabfile.org)

1. `fabric` is a python (2.5-2.7) `library` and `command-line tool`

1. for streamlining the use of `ssh`

    * for application deployment

    * for system administration tasks

### installing

1. [pip](http://pip-installer.org/)

            $ pip install fabric

1. via os's package manager, package called `fabric` or `python-fabric`

            $ sudo apt-get install fabric

1. via `conda`

            $ conda install fabric

### demo

1. hello world

    1. fabfile.py

            $ pico fabfile.py
            def hello_world():
                print 'hello world'

    1. run

            $ fab hello_world
            hello world

            Done.

1. hello world with parameter

    1. fabfile.py

            $ pico fabfile.py
            def hello_world(username):
                print 'hello world {}'.format(username)

    1. run

            $ fab hello_world:hqlgree2
            hello world hqlgree2

            Done.

1. [uname -s](http://www.fabfile.org/)

    1. fabfile.py

            $ pico fabfile.py
            from fabric.api import run

            def host_type():
                run('uname -s')

    1. run

            $ fab host_type
            No hosts found. Please specify (single) host string for connection: localhost
            [localhost] run: uname -s
            [localhost] out: Darwin
            [localhost] out: 


            Done.
            Disconnecting from localhost... done.

            $ fab host_type -H localhost,linuxbox
