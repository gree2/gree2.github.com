---
layout: post
title: "python moin in docker centos container"
description: ""
category: [python, docker]
tags: [moin, docker, centos, container, httpd, mod_wsgi, yum]
---
{% include JB/setup %}


### quick installation

1. docker container

    1. pull centos

            $ docker pull centos:6

    1. run bash

            $ docker run -it -p 80:80 --name gcentos centos:6 /bin/bash

    1. copy moin-1.9.8.tar.gz to container

            $ docker cp /c/Users/moin-1.9.8.tar.gz gcentos:/root/

1. install

    1. requirements

            # yum install httpd mod_wsgi

    1. install moinmoin

            # download and copy to vm
            # docker cp /c/Users/moin-1.9.8.tar.gz gcentos:/root/

            # tar zxvf moin-1.9.8.tar.gz
            # cd moin-1.9.8
            # python setup.py install --force --prefix /usr/local --record=intall.log

    1. copy config file

            # cd /usr/local/share/moin
            # cp server/moin.wsgi .
            # cp config/wikiconfig.py .

    1. web server config

            # vi /etc/httpd/conf/httpd.conf
            ...
            # add at the end of the file
            # MoinMoin WSGI configuration
            # you will invoke your moin wiki at the root rul
            # like http://servername/FrontPage
            WSGIScriptAlias / /usr/local/share/moin/moin.wsgi

            # create some wsgi daemons
            # use user/group same as your data dir
            WSGIDaemonProcess moin user=apache group=apache processes=5 threads=10 maximum-requests=1000 umask=0007

            # use the daemons we defined above to process requests
            WSGIProcessGroup moin

            # wsgi socket prefix
            WSGISocketPrefix /var/run/moin-wsgi

    1. verify load module statement

            # vi /etc/httpd/conf/httpd.conf
            ...
            LoadModule wsgi_module modules/mod_wsgi.so

    1. allow access

            <Directory /usr/local/share/moin>
                Order deny,allow
                Allow from all
            </Directory>

    1. wsgi stuff

            # vi /usr/local/share/moin/moin.wsgi
            ...
            # add this line to the end of a1) paragraph
            sys.path.insert(0, '/usr/local/lib/python2.6/site-packages')

            ...
            # add this line to the end of a2) paragraph
            sys.path.insert(0, '/usr/local/share/moin')

    1. security setup

            # cd /usr/local/share
            # chown -R apache:apache moin
            # chmod -R ug+rwx moin
            # chmod -R o-rwx moin

1. run

    1. apply changes

            # service httpd restart

    1. wiki config

            # vi /usr/local/share/moin/wikiconfig.py
            ...
            page_front_page = u'FrontPage'

            ...
            superuser = [u'WikiAdmin', ]

    1. lang

            login as WikiAdmin then go to
            http://192.168.99.100/LanguageSetup?action=language_setup

            pick up your lang

            then install

            then restart httpd
