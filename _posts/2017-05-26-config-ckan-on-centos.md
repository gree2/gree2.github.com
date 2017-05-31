---
layout: post
title: "config ckan on centos"
description: ""
category: [linux, setup]
tags: [centos, 7.3, ckan, virtualenv]
---
{% include JB/setup %}


### config

1. create sysadmin user

    1. activate virtual env

            $ su -s /bin/bash - ckan
            $ default/bin/activate
            $ cd /usr/lib/ckan/default/src/ckan

    1. sysadmin

            $ paster sysadmin add demo -c /etc/ckan/default/development.ini

1. create test data

    1. create-test-data

            $ paster create-test-data -c /etc/ckan/default/development.ini
