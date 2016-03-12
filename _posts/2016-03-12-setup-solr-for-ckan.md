---
layout: post
title: "setup solr for ckan"
description: ""
category: [setup]
tags: [solr, ckan, jetty]
---
{% include JB/setup %}


### steps

1. single core setup

    1. edit jetty config file

            # 0. install
            $ sudo apt-get install -y postgresql solr-jetty

            # 1. config
            $ sudo pico /etc/default/jetty

            NO_START=0           # line 4
            JETTY_HOST=127.0.0.1 # line 16
            JETTY_PORT=8983      # line 19

            # 2. note JETTY_HOST
            ip or 0.0.0.0

            # 3. start jetty server
            $ sudo service jetty start

            # 4. in web browser
            http://localhost:8983/solr/

            # 5. if could not start jetty
            # setup JAVA_HOME
            $ sudo pico /etc/default/jetty
            JAVA_HOME=/usr/lib/jvm/java-6-openjdk-amd64/
            # or
            JAVA_HOME=/usr/lib/jvm/java-6-openjdk-i386/

    1. replace default `schema.xml` file with a symlink

            # 1. replace
            $ sudo mv /etc/solr/conf/schema.xml /etc/solr/conf/schema.xml.bak
            $ sudo ln -s /usr/lib/ckan/default/src/ckan/ckan/config/solr/schema.xml /etc/solr/conf/schema.xml

            # 2. restart solr
            $ sudo service jetty restart

            # 3. check
            http://localhost:8983/solr/

    1. change [solr_url](http://docs.ckan.org/en/latest/maintaining/configuration.html#solr-url) setting in ckan config file

            $ sudo pico /etc/ckan/default/production.ini
            solr_url = http://127.0.0.1:8983/solr

1. [multicore solr setup](http://docs.ckan.org/en/latest/maintaining/solr-multicore.html)

### fixed

1. [Wrong path of rotatelogs in /etc/init.d/jetty](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=719875)