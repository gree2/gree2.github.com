---
layout: post
title: "docker up and running tomcat"
description: ""
category:
tags: []
---
{% include JB/setup %}


### docs

1. version

        $ docker pull tomcat:7.0.82

1. how to

    1. run default server CMD ["catalina.sh", "run"]

            $ docker run -it --rm tomcat:7.0.82
            $ open container-ip:8080

            # ports
            8005 server shutdown
            8080 connector protocol HTTP/1.1
            8443 connector protocol org.apache.coyote.http11.Http11Protocol
            8009 connector protocol AJP/1.3

            $ docker run -it --rm                   \
            -p 6765:8080 -p 6443:8443 -p 6009:8009  \
            -v $(pwd)/conf/:/usr/local/tomcat/conf/ \
            -v $(pwd)/target/demo.war:/usr/local/tomcat/webapps/demo.war \
            tomcat:7.0.82

    1. default tomcat env in image

            # for version 7 & 8
            CATALINA_BASE:   /usr/local/tomcat
            CATALINA_HOME:   /usr/local/tomcat
            CATALINA_TMPDIR: /usr/local/tomcat/tmp
            JRE_HOME:        /usr
            CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar

            # for version 6
            CATALINA_BASE:   /usr/local/tomcat
            CATALINA_HOME:   /usr/local/tomcat
            CATALINA_TMPDIR: /usr/local/tomcat/temp
            JRE_HOME:        /usr
            CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar

    1. config file

            /usr/local/tomcat/conf/

            default no user is included in
            manager-gui <= required to operate /manager/html web app

            if you wish define a user in tomcat-users.xml

1. demo

        $ docker run -it --rm                   \
        -p 6765:8080 -p 6443:8443 -p 6009:8009  \
        -v $(pwd)/conf/:/usr/local/tomcat/conf/ \
        -v $(pwd)/target/demo.war:/usr/local/tomcat/webapps/demo.war \
        tomcat:7.0.82

### references

1. [https://store.docker.com/images/tomcat](https://store.docker.com/images/tomcat)
