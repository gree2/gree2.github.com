---
layout: post
title: "supervisor on ubuntu"
description: ""
category: [auto, linux, ubuntu]
tags: [supervisor, ubuntu, auto, install, update]
---
{% include JB/setup %}


### supervisor

1. install

            $ sudo apt-get update
            $ sudo apt-get install supervisor

1. config

    1. supervisor's config file

            $ cd /etc/supervisor
            $ cat supervisord.conf
            ; supervisor config file

            [unix_http_server]
            file=/var/run/supervisor.sock   ; (the path to the socket file)
            chmod=0700                       ; sockef file mode (default 0700)

            [supervisord]
            logfile=/var/log/supervisor/supervisord.log ; (main log file;default $CWD/supervisord.log)
            pidfile=/var/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
            childlogdir=/var/log/supervisor            ; ('AUTO' child log dir, default $TEMP)

            ; the below section must remain in the config file for RPC
            ; (supervisorctl/web interface) to work, additional interfaces may be
            ; added by defining them in separate rpcinterface: sections
            [rpcinterface:supervisor]
            supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

            [supervisorctl]
            serverurl=unix:///var/run/supervisor.sock ; use a unix:// URL  for a unix socket

            ; The [include] section can just contain the "files" setting.  This
            ; setting can list multiple files (separated by whitespace or
            ; newlines).  It can also contain wildcards.  The filenames are
            ; interpreted as relative to this file.  Included files *cannot*
            ; include files themselves.

            [include]
            files = /etc/supervisor/conf.d/*.conf

    1. [adding a program](http://supervisord.org/running.html#adding-a-program)

            # add to supervisord.conf
            $ sudo pico supervisord.conf
            ...
            [program:foo]
            command=/bin/cat

    1. [program:x] section settings

        1. command

                command=/path/to/program
                command=program
                command=program foo bar
                command=program -p "foo bar"

        1. autostart

                if true this program will start automatically when supervisord is started
                default: true

        1. autorestart

                specifies if supervisord should automatically restart a process if it exits
                default: unexpected (false, true, unexpected)

        1. directory

                when supervisord daemonizs switch to this directory
                default: do not cd


### usage

1. docker-storm

    1. [config-supervisord.sh](https://github.com/gree2/hello-docker/blob/master/docker-storm/storm/config-supervisord.sh)

            echo [program:storm-$1]    | tee -a /etc/supervisor/conf.d/storm-$1.conf
            echo command=storm $1      | tee -a /etc/supervisor/conf.d/storm-$1.conf
            echo directory=/home/storm | tee -a /etc/supervisor/conf.d/storm-$1.conf
            echo autorestart=true      | tee -a /etc/supervisor/conf.d/storm-$1.conf
            echo user=root             | tee -a /etc/supervisor/conf.d/storm-$1.conf

    1. when build docker images

            # in storm-nimbus's dockerfile
            RUN /usr/bin/config-supervisord.sh nimbus
            RUN /usr/bin/config-supervisord.sh drpc

            # in storm-supervisor's dockerfile
            RUN /usr/bin/config-supervisord.sh supervisor
            RUN /usr/bin/config-supervisord.sh logviewer

            # in storm-ui's dockerfile
            RUN /usr/bin/config-supervisord.sh ui

### reference

1. [http://supervisord.org](http://supervisord.org)

1. [docker-storm](https://github.com/gree2/hello-docker/tree/master/docker-storm)
