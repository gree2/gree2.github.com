---
layout: post
title: "apache solr getting started"
description: ""
category: [bigdata]
tags: [solr, lucene]
---
{% include JB/setup %}


### info

1. [http://lucene.apache.org/solr/](http://lucene.apache.org/solr/)

    1. solr is highly reliable

    1. scalable and fault tolerant

    1. providing distributed indexing

    1. replication and load-balanced querying

    1. automated failover and recovery

    1. centralized configuration and more

### install

1. manual

    1. [download](http://www.apache.org/dyn/closer.cgi/lucene/solr/5.2.0)

    1. unzip and run

            $ tar zxf solr-5.2.0.tgz

    1. then run

1. use homebrew

            $ brew install solr
            ==> Downloading https://www.apache.org/dyn/closer.cgi?path=lucene/solr/5.1.0/solr-5.1.0.tgz
            ==> Best Mirror http://apache.fayea.com/lucene/solr/5.1.0/solr-5.1.0.tgz
            ######################################################################## 100.0%
            ==> Caveats
            To have launchd start solr at login:
                ln -sfv /usr/local/opt/solr/*.plist ~/Library/LaunchAgents
            Then to load solr now:
                launchctl load ~/Library/LaunchAgents/homebrew.mxcl.solr.plist
            Or, if you don't want/need launchctl, you can just run:
                solr start
            ==> Summary
            🍺  /usr/local/Cellar/solr/5.1.0: 4647 files, 199M, built in 18.4 minutes

1. [use shell](https://gist.github.com/bloveridge/591964)

            $ pico solr_install.sh
            # install solr
            brew install solr

            # create base solr index directory
            mkdir -p /data/solr

            # make sure write to solr logs
            sudo chown -R `whoami` /usr/local/Cellar/solr/

            # running
            solr /full/path/to/multicore

### running solr

1. command options and admin console

    1. start the server

            # linux or unix
            $ solr start
            Waiting to see Solr listening on port 8983 [-]  
            Started Solr server on port 8983 (pid=22992). Happy searching!

            $ solr start

            Solr home directory /usr/local/solr/libexec must contain a solr.xml file!

            $ pwd
            /usr/local/solr/server/solr
            $ ls
            README.txt configsets demo       solr.xml   zoo.cfg
            $ cp solr.xml ../../libexec/
            $ solr start
            Waiting to see Solr listening on port 8983 [-]  
            Started Solr server on port 8983 (pid=24935). Happy searching!

            # windows
            > solr.cmd start

            # start solr in the foreground
            $ solr start -f

            # on windows
            > solr.cmd start -f

    1. script options

            $ solr -help

            $ solr start -help

    1. check if solr is running

            $ solr status
            Found 1 Solr nodes: 

            Solr process 22992 running on port 8983
            {
              "solr_home":"/usr/local/Cellar/solr/5.1.0/server/solr/",
              "version":"5.1.0 1672403 - timpotter - 2015-04-09 10:37:54",
              "startTime":"2015-06-09T09:03:46.389Z",
              "uptime":"0 days, 0 hours, 7 minutes, 23 seconds",
              "memory":"21.6 MB (%4.4) of 491 MB"}

    1. stop the server

            # in the foreground
            # use `ctrl+c` to stop

            # running in the background
            # specify the port solr is listening on
            $ solr stop -p 8983

            $ solr stop
            Sending stop command to Solr running on port 8983 ... waiting 5 seconds to allow Jetty process 22992 to stop gracefully.

            # stop all running solr instances
            $ solr stop -all

    1. start solr with a example configuration

            # solr provides a number of examples
            # launch the example using -e flag
            $ solr -e techproducts

            # available examples
            # techproducts/dih/schemaless/cloud

    1. web browser admin console

            http://localhost:8983/solr/

1. create a core

            # if not start with an example configuration
            # need to create a core
            # in order to be able to index and search
            $ solr create -c demo
            Failed to determine the port of a local Solr instance, cannot create demo!

            $ solr create -c demo

            Setup new core instance directory:
            /usr/local/Cellar/solr/5.1.0/server/solr/demo

            Creating new core 'demo' using command:
            http://localhost:8983/solr/admin/cores?action=CREATE&name=demo&instanceDir=demo

            {
              "responseHeader":{
                "status":0,
                "QTime":857},
              "core":"demo"}

            $ solr create -help

1. add documents

    1. use `post` script

            $ libexec/bin/post -c demo example/exampledocs/*.xml
            /Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home/bin/java -classpath /usr/local/solr/libexec/dist/solr-core-5.1.0.jar -Dauto=yes -Dc=demo -Ddata=files org.apache.solr.util.SimplePostTool example/exampledocs/gb18030-example.xml example/exampledocs/hd.xml example/exampledocs/ipod_other.xml example/exampledocs/ipod_video.xml example/exampledocs/manufacturers.xml example/exampledocs/mem.xml example/exampledocs/money.xml example/exampledocs/monitor.xml example/exampledocs/monitor2.xml example/exampledocs/mp500.xml example/exampledocs/sd500.xml example/exampledocs/solr.xml example/exampledocs/utf8-example.xml example/exampledocs/vidcard.xml
            SimplePostTool version 5.0.0
            Posting files to [base] url http://localhost:8983/solr/demo/update...
            Entering auto mode. File endings considered are xml,json,csv,pdf,doc,docx,ppt,pptx,xls,xlsx,odt,odp,ods,ott,otp,ots,rtf,htm,html,txt,log
            POSTing file gb18030-example.xml (application/xml) to [base]
            POSTing file hd.xml (application/xml) to [base]
            POSTing file ipod_other.xml (application/xml) to [base]
            POSTing file ipod_video.xml (application/xml) to [base]
            POSTing file manufacturers.xml (application/xml) to [base]
            POSTing file mem.xml (application/xml) to [base]
            POSTing file money.xml (application/xml) to [base]
            POSTing file monitor.xml (application/xml) to [base]
            POSTing file monitor2.xml (application/xml) to [base]
            POSTing file mp500.xml (application/xml) to [base]
            POSTing file sd500.xml (application/xml) to [base]
            POSTing file solr.xml (application/xml) to [base]
            POSTing file utf8-example.xml (application/xml) to [base]
            POSTing file vidcard.xml (application/xml) to [base]
            14 files indexed.
            COMMITting Solr index changes to http://localhost:8983/solr/demo/update...
            Time spent: 0:00:00.384

