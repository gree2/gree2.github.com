---
layout: post
title: "centos 6.5 install nginx"
description: ""
category: [linux]
tags: [centos, 6.5, nginx, proxy, config]
---
{% include JB/setup %}


### steps

1. add nginx repo

    1. create /etc/yum.repos.d/nginx.repo

            # method 1
            $ nano /etc/yum.repos.d/nginx.repo

            [nginx]
            name=nginx repo
            baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
            gpgcheck=0
            enabled=1

            # method 2
            echo -e "[nginx]\nname=nginx repo\nbaseurl=http://nginx.org/packages/centos/\$releasever/\$basearch\nenabled=1\ngpgcheck=0" > /etc/yum.repos.d/nginx.repo

    1. install after nginx repo setup

            $ yum install nginx

1. config

    1. auto start and shutdow

            $ chkconfig nginx on

    1. config file

            # reverse proxy
            $ nano /etc/nginx/nginx.conf

### reference

1. [https://github.com/rharmonson/richtech/wiki/Installing-Nginx-on-CentOS-6.5-Minimal-x86_64](https://github.com/rharmonson/richtech/wiki/Installing-Nginx-on-CentOS-6.5-Minimal-x86_64)

1. [http://wiki.nginx.org/Install#Official_Red_Hat.2FCentOS_packages](http://wiki.nginx.org/Install#Official_Red_Hat.2FCentOS_packages)
