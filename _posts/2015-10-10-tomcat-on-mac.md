---
layout: post
title: "tomcat on mac"
description: ""
category: [web]
tags: [tomcat, username, password, port]
---
{% include JB/setup %}


### install

1. use brew

            $ brew install tomcat
            $ cd /usr/local
            $ ln -s Cellar/tomcat/8.0.24 tomcat

### usage

1. start/stop tomcat

    1. start

            # on linux/mac
            $ cd /usr/local/tomcat/libexec
            $ sh startup.sh

            # on windows
            > startup.bat

            # test
            http://localhost:8080

    1. stop

            # on linux/mac
            $ sh shutdown.sh

            # on windows
            > shutdown.bat

1. catalina

    1. help

            $ which catalina
            /usr/local/bin/catalina
            $ ./catalina 
            Using CATALINA_BASE:   /usr/local/Cellar/tomcat/8.0.24/libexec
            Using CATALINA_HOME:   /usr/local/Cellar/tomcat/8.0.24/libexec
            Using CATALINA_TMPDIR: /usr/local/Cellar/tomcat/8.0.24/libexec/temp
            Using JRE_HOME:        /Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home
            Using CLASSPATH:       /usr/local/Cellar/tomcat/8.0.24/libexec/bin/bootstrap.jar:/usr/local/Cellar/tomcat/8.0.24/libexec/bin/tomcat-juli.jar
            Usage: catalina.sh ( commands ... )
            commands:
              debug             Start Catalina in a debugger
              debug -security   Debug Catalina with a security manager
              jpda start        Start Catalina under JPDA debugger
              run               Start Catalina in the current window
              run -security     Start in the current window with security manager
              start             Start Catalina in a separate window
              start -security   Start in a separate window with security manager
              stop              Stop Catalina, waiting up to 5 seconds for the process to end
              stop n            Stop Catalina, waiting up to n seconds for the process to end
              stop -force       Stop Catalina, wait up to 5 seconds and then use kill -KILL if still running
              stop n -force     Stop Catalina, wait up to n seconds and then use kill -KILL if still running
              configtest        Run a basic syntax check on server.xml - check exit code for result
              version           What version of tomcat are you running?
            Note: Waiting for the process to end and use of the -force option require that $CATALINA_PID is defined

    1. start catalina in the current window

            $ catalina run
            ...
            10-Oct-2015 22:48:50.264 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["http-nio-8080"]
            10-Oct-2015 22:48:50.268 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["ajp-nio-8009"]
            10-Oct-2015 22:48:50.268 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in 594 ms

1. use `/manager/html` web application

    1. setup `manager-gui` user

            $ cd /usr/local/tomcat/libexec/conf
            $ pico tomcat-users.xml
            <?xml version='1.0' encoding='utf-8'?>
            <tomcat-users xmlns="http://tomcat.apache.org/xml"
                          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                          xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
                          version="1.0">
            <!--
              NOTE:  By default, no user is included in the "manager-gui" role required
              to operate the "/manager/html" web application.  If you wish to use this app,
              you must define such a user - the username and password are arbitrary.
            -->
            <!--
              NOTE:  The sample user and role entries below are wrapped in a comment
              and thus are ignored when reading this file. Do not forget to remove
              <!.. ..> that surrounds them.
            -->
            <!--
              <role rolename="tomcat"/>
              <role rolename="role1"/>
              <user username="tomcat" password="tomcat" roles="tomcat"/>
              <user username="both" password="tomcat" roles="tomcat,role1"/>
              <user username="role1" password="tomcat" roles="role1"/>
            -->
              <user username="admin" password="admin" roles="manager-gui"/>
            </tomcat-users>

    1. visit [http://localhost:8080/manager/html](http://localhost:8080/manager/html)

1. change port 8080

    1. edit `libexec/conf/server.xml`

            $ pico server.xml
            # press `ctrl+w` input `8080` hit `enter`
            <Connector port="8080" protocol="HTTP/1.1"
                connectionTimeout="20000"
                redirectPort="8443" />
            # change `8080` to whatever you want
