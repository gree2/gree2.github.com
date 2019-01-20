---
layout: post
title: "docker set container timezone and localtime"
description: ""
category: [docker]
tags: [container, timezone, localtime, exec, bash, debian]
---
{% include JB/setup %}


### steps

1. docker images

        tomcat 7.0.82
        nginx  1.13.8

1. docker container timezone and localtime files

    1. tomcat

            $ docker exec -it mytc bash
            $ cat /etc/issue.net
            Debian GNU/Linux 8

            $ cat /etc/timezone
            Asia/Shanghai

            $ ls -l /etc/localtime
            lrwxrwxrwx. 1 root root 33 Jun 12 11:05 /etc/localtime -> /usr/share/zoneinfo/Asia/Shanghai

    1. nginx

            $ docker exec -it myngx bash
            $ cat /etc/issue.net
            Debian GNU/Linux 9

            $ cat /etc/timezone
            Asia/Shanghai

            $ ls -l /etc/localtime
            lrwxrwxrwx. 1 root root 23 Jun 12 19:27 /etc/localtime -> /usr/share/zoneinfo/Asia/Shanghai

1. docker setup timezone and localtime

    1. docker command

            $ docker exec mytc bash -c 'ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime'
            $ docker exec mytc bash -c 'echo Asia/Shanghai > /etc/timezone'

    1. ansible playbook

            # adds these 2 tasks after container startup

            - name: link localtime
              command: "docker exec mytc bash -c 'ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime'"
              ignore_errors: yes

            - name: set timezone
              command: "docker exec mytc bash -c 'echo Asia/Shanghai > /etc/timezone'"
              ignore_errors: yes

### references

1. [docker exec write text to file in container](https://stackoverflow.com/questions/35703317/docker-exec-write-text-to-file-in-container)
