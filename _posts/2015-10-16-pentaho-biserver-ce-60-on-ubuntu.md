---
layout: post
title: "pentaho biserver ce 6.0 on ubuntu"
description: ""
category: [bi]
tags: [pentaho, biserver, 6.0, ubuntu, puc]
---
{% include JB/setup %}


### setup

1. [download pentaho biserver](http://jaist.dl.sourceforge.net/project/pentaho/Business%20Intelligence%20Server/6.0/biserver-ce-6.0.0.0-353.zip)

1. [setup ubuntu with oracle jdk]({% post_url 2015-09-24-install-oracle-jdk-on-ubuntu %})

1. setup pentaho biserver

### setup pentaho biserver

1. steps

    1. cp `biserver-ce-6.0.0.0-353.zip` to ubuntu

            # on host mac
            $ scp biserver-ce-6.0.0.0-353.zip node@pentaho:/home/node/
            Warning: the RSA host key for 'pentaho' differs from the key for the IP address '192.168.1.100'
            Offending key for IP in /Users/hqlgree2/.ssh/known_hosts:35
            Matching host key in /Users/hqlgree2/.ssh/known_hosts:38
            Are you sure you want to continue connecting (yes/no)? yes
            node@pentaho's password: 
            biserver-ce-6.0.0.0-353.zip                               100%  930MB  71.5MB/s   00:13

    1. ssh to node pentaho

            $ ssh node@pentaho
            Warning: the RSA host key for 'pentaho' differs from the key for the IP address '192.168.1.100'
            Offending key for IP in /Users/hqlgree2/.ssh/known_hosts:35
            Matching host key in /Users/hqlgree2/.ssh/known_hosts:38
            Are you sure you want to continue connecting (yes/no)? yes
            node@pentaho's password: 
            Welcome to Ubuntu 14.04.1 LTS (GNU/Linux 3.13.0-32-generic x86_64)

             * Documentation:  https://help.ubuntu.com/
            Last login: Fri Oct 16 13:58:58 2015

    1. unzip `biserver-ce-6.0.0.0-353.zip`

            # on node pentaho
            $ unzip biserver-ce-6.0.0.0-353.zip

    1. edit `start-pentaho.sh` check my [post]({% post_url 2015-10-08-pentaho-bi-server-report-designer-on-mac %})

            # on node pentaho
            $ pico start-pentaho.sh
            # add this to `CATALINA_OPTS`
            -Dh2.bindAddress=localhost

1. usage

    1. start biserver

            $ cd biserver-ce
            $ sh start-pentaho.sh
            DEBUG: Using JAVA_HOME
            DEBUG: _PENTAHO_JAVA_HOME=/usr/local/java/jdk1.7.0_79
            DEBUG: _PENTAHO_JAVA=/usr/local/java/jdk1.7.0_79/bin/java
            --------------------------------------------------------------------------------------------
            The Pentaho BI Platform now contains a version checker that will notify you
            when newer versions of the software are available. The version checker is enabled by default.
            For information on what the version checker does, why it is beneficial, and how it works see:
            http://wiki.pentaho.com/display/ServerDoc2x/Version+Checker
            Press Enter to continue, or type cancel or Ctrl-C to prevent the server from starting.
            You will only be prompted once with this question.
            --------------------------------------------------------------------------------------------
            [OK]:

            Using CATALINA_BASE:   /home/node/biserver-ce-6.0/tomcat
            Using CATALINA_HOME:   /home/node/biserver-ce-6.0/tomcat
            Using CATALINA_TMPDIR: /home/node/biserver-ce-6.0/tomcat/temp
            Using JRE_HOME:        /usr/local/java/jdk1.7.0_79/jre
            Using CLASSPATH:       /home/node/biserver-ce-6.0/tomcat/bin/bootstrap.jar:/home/node/biserver-ce-6.0/tomcat/bin/tomcat-juli.jar
            Tomcat started.

    1. check port and pid

            $ netstat -antp
            (Not all processes could be identified, non-owned process info
             will not be shown, you would have to be root to see it all.)
            Active Internet connections (servers and established)
            Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
            tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -               
            tcp        0     36 192.168.1.100:22        192.168.1.101:59894     ESTABLISHED -               
            tcp6       0      0 :::8009                 :::*                    LISTEN      1066/java       
            tcp6       0      0 :::8080                 :::*                    LISTEN      1066/java       
            tcp6       0      0 :::22                   :::*                    LISTEN      -               

    1. visit [pentaho user console - puc](http://192.168.1.100:8080/pentaho/Home) from your favourite browser
