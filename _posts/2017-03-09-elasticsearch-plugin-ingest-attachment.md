---
layout: post
title: "elasticsearch plugin ingest attachment"
description: ""
category: [es]
tags: [elasticsearch, plugin, ingest-attachment, install, remove]
---
{% include JB/setup %}


### [ingest-attachment](https://www.elastic.co/guide/en/elasticsearch/plugins/5.1/ingest-attachment.html)

1. info

    1. extract file attachments using [tika](http://lucene.apache.org/tika/)

    1. `source` field must be `base64 encoded binary`

1. using

    1. install

            $ bin/elasticsearch-plugin install ingest-attachments

    1. offline install [download ingest-attachment-5.1.2.zip](https://artifacts.elastic.co/downloads/elasticsearch-plugins/ingest-attachment/ingest-attachment-5.1.2.zip)

            # on host machine
            $ cd Download; python -m SimpleHTTPServer

            # on host machine
            $ docker exec -it ges bash
            root@da5965afc265:/usr/share/elasticsearch# bin/elasticsearch-plugin install http://192.168.100.2:8000/ingest-attachment-5.1.2.zip
            -> Downloading http://192.168.100.2:8000/ingest-attachment-5.1.2.zip
            [=================================================] 100%
            Exception in thread "main" java.lang.IllegalArgumentException: Plugin [ingest-attachment] is incompatible with Elasticsearch [5.1.1]. Was designed for version [5.1.2]
                at org.elasticsearch.plugins.PluginInfo.readFromProperties(PluginInfo.java:108)
                at org.elasticsearch.plugins.InstallPluginCommand.verify(InstallPluginCommand.java:421)
                at org.elasticsearch.plugins.InstallPluginCommand.install(InstallPluginCommand.java:474)
                at org.elasticsearch.plugins.InstallPluginCommand.execute(InstallPluginCommand.java:212)
                at org.elasticsearch.plugins.InstallPluginCommand.execute(InstallPluginCommand.java:195)
                at org.elasticsearch.cli.SettingCommand.execute(SettingCommand.java:54)
                at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:96)
                at org.elasticsearch.cli.MultiCommand.execute(MultiCommand.java:69)
                at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:96)
                at org.elasticsearch.cli.Command.main(Command.java:62)
                at org.elasticsearch.plugins.PluginCli.main(PluginCli.java:42)

            root@da5965afc265:/usr/share/elasticsearch# bin/elasticsearch-plugin install http://192.168.100.2:8000/ingest-attachment-5.1.1.zip
            -> Downloading http://192.168.100.2:8000/ingest-attachment-5.1.1.zip
            [=================================================] 100%
            @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
            @     WARNING: plugin requires additional permissions     @
            @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
            * java.lang.RuntimePermission getClassLoader
            * java.lang.reflect.ReflectPermission suppressAccessChecks
            * java.security.SecurityPermission createAccessControlContext
            * java.security.SecurityPermission insertProvider
            * java.security.SecurityPermission putProviderProperty.BC
            See http://docs.oracle.com/javase/8/docs/technotes/guides/security/permissions.html
            for descriptions of what these permissions allow and the associated risks.

            Continue with installation? [y/N]y
            -> Installed ingest-attachment

            root@da5965afc265:/usr/share/elasticsearch# bin/elasticsearch-plugin install http://192.168.100.2:8000/analysis-smartcn-5.1.1.zip

    1. removal

            $ bin/elasticsearch-plugin remove ingest-attachments

### [elasticsearch-mapper-attachments-2.0.0](https://download.elasticsearch.org/elasticsearch/elasticsearch-mapper-attachments/elasticsearch-mapper-attachments-2.0.0.zip)

1. install special version

            # transwarp installation path
            $ cd /usr/lib/elasticsearch/elasticsearch-2.0.0-transwarp
            # https://download.elastic.co/elasticsearch/elasticsearch-mapper-attachments/elasticsearch-mapper-attachments-3.0.2.zip
            $ bin/plugin install elasticsearch/elasticsearch-mapper-attachments/3.0.2

### demo

1. [jupyter notebook](http://beenje.github.io/blog/posts/parsing-and-indexing-pdf-in-python/) passed

1. [curl demo](https://hustbill.wordpress.com/2015/09/23/index-a-ms-office-documents-to-elasticsearch/) not pass

### references

1. [mapper-attachments](https://www.elastic.co/guide/en/elasticsearch/plugins/current/mapper-attachments.html)

            The mapper-attachments plugin has been replaced by the ingest-attachment plugin

1. [https://github.com/elastic/elasticsearch-mapper-attachments#installation](https://github.com/elastic/elasticsearch-mapper-attachments#installation)

            Elasticsearch   Attachments Plugin  Docs
            2.0.0           3.0.2               3.0.2