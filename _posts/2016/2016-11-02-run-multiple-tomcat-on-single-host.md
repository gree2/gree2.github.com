---
layout: post
title: "run multiple tomcat on single host"
description: ""
category: [devops]
tags: [tomcat, http, port, jvm, shutdown]
---
{% include JB/setup %}


### setup

1. change 3 ports in file `conf/server.xml`

    1. http port default 8080

            <Connector port=”8080” protocol=”HTTP/1.1″
            connectionTimeout=”20000″
            redirectPort=”8443″ URIEncoding=”gb2312″/>

    1. shutdown port default 8005

            <Server port=”8005” shutdown=”SHUTDOWN”>

    1. jvm port default 8009

        <Connector port=”8009” protocol=”AJP/1.3″ redirectPort=”8443″ />
