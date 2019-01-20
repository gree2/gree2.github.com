---
layout: post
title: "tomcat https setup"
description: ""
category: [devops]
tags: [tomcat, keystore, keytool, ssl, openssl, listener, connector]
---
{% include JB/setup %}


### steps

1. gen cert from [https://console.cloud.tencent.com](https://console.cloud.tencent.com/ssl)

1. conf tomcat

    1. server.xml

            <Connector port="8443"
            maxHttpHeaderSize="8192"
            maxThreads="150"
            minSpareThreads="25"
            maxSpareThreads="75"
            enableLookups="false"
            disableUploadTimeout="true"
            acceptCount="100"
            scheme="https"
            secure="true"
            SSLEnabled="true"
            clientAuth="false"
            sslProtocol="TLS"
            keystoreFile="/path/to/gree2.github.com.jks"
            keystorePass="your_keystore_password" />

    1. start tomcat

            $ ./startup.sh

            # error in log file
            ...
            Connector attribute SSLCertificateFile must be defined when using SSL with APR
            ...


### keytools

1. gen p12 file

        $ openssl pkcs12 -export -in 2_gree2.github.io.crt -inkey 3_gree2.github.io.key -out 3_gree2.github.io.p12 -name 3_gree2.github.io

1. gen jks file

        $ keytool -importkeystore -srckeystore 3_gree2.github.io.p12 -srcstoretype PKCS12 -destkeystore 3_gree2.github.io.jks

1. gen public key optional

        $ openssl rsa -in 3_gree2.github.io.key -out 3_gree2.github.io_public.key

1. add ca crt

        $ keytool -keystore 3_gree2.github.io.jks -import -trustcacerts -alias alias -file ca.crt

1. your can check your password with this command

        $ keytool -list -v -keystore 3_gree2.github.io.jks

### fixed

1. Connector attribute SSLCertificateFile must be defined when using SSL with APR

    1. [fixed](https://stackoverflow.com/questions/6306314/tomcat-7-ssl-failed)

            # comment out this line
            <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
