---
layout: post
title: "tesseract ocr parser within tika"
description: ""
category: [ocr]
tags: [tesseract, ocr, tika, tika-app, tika-server, tika-python]
---
{% include JB/setup %}


### steps

1. install

        $ brew install tesseract --all-languages --with-serial-num-pack
        Installing dependencies for tesseract: autoconf, autoconf-archive, automake, leptonica
        ...
        Warning: tesseract: --all-languages was deprecated; using --with-all-languages instead!
        ...

1. using tika and tesseract

    1. use tesseract

            $ tesseract -psm 3 /path/to/tiff/file.tiff out.txt
            $ cat out.txt

    1. use tika

            $ tika -t /path/to/tiff/file.tiff

1. using tika server and tesseract

    1. start tika server

            $ java -jar /path/to/tika-server-1.7-SNAPSHOT.jar

    1. curl request

            $ curl -T /path/to/tiff/file.tiff http://localhost:9998/tika --header "Content-type: image/tiff"

    1. using different language models

            # single language
            $ curl -T /path/to/tiff/image.jpg http://localhost:998/tika --header "X-Tika-OCRLanguage: eng"
            # multiple languages
            $ curl -T /path/to/tiff/image.jpg http://localhost:998/tika --header "X-Tika-OCRLanguage: eng+fra"

1. [docker-tikaserver](https://hub.docker.com/r/logicalspark/docker-tikaserver/) Dockerfile on [github](https://github.com/LogicalSpark/docker-tikaserver)

    1. pull

            $ docker pull logicalspark/docker-tikaserver

    1. run

            $ docker run --rm -d -p 9998:9998 logicalspark/docker-tikaserver

1. [command-line usage](https://github.com/tesseract-ocr/tesseract/wiki/Command-Line-Usage)

    1. set `TESSDATA_PREFIX`

            $ cd /usr/local/Cellar/tesseract/3.05.00/share/tessdata
            $ export TESSDATA_PREFIX="`pwd`"

    1. chi_sim

            $ tesseract ./test.png ./test -l chi_sim
            Tesseract Open Source OCR Engine v3.05.00 with Leptonica
            $ cat text.txt
            ...
            orc output of test.png file
            ...

1. running the tika server

    1. java -jar

            $ java -jar tika-server-1.14.jar
            ...

    1. change host name and port number

            $ java -jar tika-server-1.14.jar --host=intranet.local --port=9999
            Mar 09, 2017 9:35:55 AM org.apache.tika.server.TikaServerCli main
            INFO: Starting Apache Tika 1.14 server
            Mar 09, 2017 9:35:56 AM org.apache.cxf.endpoint.ServerImpl initDestination
            INFO: Setting the server's publish address to be http://localhost:9999/
            Mar 09, 2017 9:35:56 AM org.slf4j.impl.JCLLoggerAdapter info
            INFO: jetty-8.y.z-SNAPSHOT
            Mar 09, 2017 9:35:56 AM org.slf4j.impl.JCLLoggerAdapter info
            INFO: Started SelectChannelConnector@localhost:9999
            Mar 09, 2017 9:35:56 AM org.apache.tika.server.TikaServerCli main
            INFO: Started

    1. visit [http://localhost:9999/](http://localhost:9999/)

            Welcome to the Apache Tika 1.14 Server

            For endpoints, please see https://wiki.apache.org/tika/TikaJAXRS and http://tika.apache.org/1.14/miredot/index.html

            PUT /detect/stream
            Class: org.apache.tika.server.resource.DetectorResource
            Method: detect
            Produces: text/plain
            GET /detectors
            Class: org.apache.tika.server.resource.TikaDetectors
            Method: getDectorsHTML
            Produces: text/html
