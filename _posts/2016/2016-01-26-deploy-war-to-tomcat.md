---
layout: post
title: "deploy war to tomcat"
description: ""
category: [web]
tags: [tomcat]
---
{% include JB/setup %}


### demo

1. steps

    1. copy `war` file to tomcat's `webapps` folder

            # your war filename
            my-first-web-app.war

    1. restart tomcat

    1. visit [my-first-web-app](http://localhost:8080/)

### demo

1. steps

    1. copy `war` file to tomcat's `webapps` folder

    1. copy jar files to tomcat's `lib` folder

    1. edit tomcat's `server.xml` under conf folder

            # in element <Host></Host>
            # add a Context node
            <Context path="/demo" docBase="demo" debug="0" privileged="true" />

            # note
            # 1. path:    after deploy you can visit http://localhost/demo
            # 2. docBase: war file name, tomcat will unzip it for you

    1. restart tomcat

    1. done
