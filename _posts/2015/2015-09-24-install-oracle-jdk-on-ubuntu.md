---
layout: post
title: "install oracle jdk on ubuntu"
description: ""
category: [ubuntu]
tags: [oracle, jdk, ubuntu]
---
{% include JB/setup %}


### solution

1. reference

    1. [wikihow](http://www.wikihow.com/Install-Oracle-Java-on-Ubuntu-Linux)

1. steps

    1. check java version

            $ java -version

    1. remove openjdk/jre

            $ sudo apt-get purge openjdk-\*

    1. copy jdk-7u79-linux-x64.tar.gz to /usr/local/java

            $ sudo cp -r jdk-7u79-linux-x64.tar.gz /usr/local/java

    1. unpack

            $ sudo tar zxf jdk-7u79-linux-x64.tar.gz

    1. double-check

            $ ls -a
            jdk1.7.0_79

    1. edit /etc/profile

            # at the end of file
            $ pico /etc/profile
            JAVA_HOME=/usr/local/java/jdk1.7.0_79
            JRE_HOME=$JAVA_HOME/jre
            PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
            export JAVA_HOME
            export JRE_HOME
            export PATH

    1. inform os where oracle jdk/jre located

            $ sudo update-alternatives --install "/usr/bin/java" "java" "/usr/local/java/jdk1.7.0_79/bin/java" 1
            $ sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/local/java/jdk1.7.0_79/bin/javac" 1
            $ sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/local/java/jdk1.7.0_79/bin/javaws" 1

    1. inform os that oracle jdk/jre is default java

            $ sudo update-alternatives --set java /usr/local/java/jdk1.7.0_79/bin/java
            $ sudo update-alternatives --set javac /usr/local/java/jdk1.7.0_79/bin/javac
            $ sudo update-alternatives --set javaws /usr/local/java/jdk1.7.0_79/bin/javaws

    1. reload system wide PATH

            $ . /etc/profile
            $ java -version

    1. reboot

            $ sudo reboot