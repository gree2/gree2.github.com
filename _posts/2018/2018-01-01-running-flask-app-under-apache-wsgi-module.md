---
layout: post
title: "running flask app under apache wsgi module"
description: ""
category: [python]
tags: [apache, httpd, wsgi, flask, mod_wsgi]
---
{% include JB/setup %}


### steps

1. install apache with brew

    1. install

            $ brew install apache-httpd
            ...
            DocumentRoot is /usr/local/var/www
            ... default ports have been set in
            /usr/local/etc/httpd/httpd.conf to 8080
            /usr/local/etc/httpd/extra/httpd-ssl.conf to 8443
            ...

    1. to have launchd start httpd now & restart at login

            $ brew services start httpd

    1. if don't need a background service just run

            $ apachectl start
            AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using gree2.local. Set the 'ServerName' directive globally to suppress this message

            $ nano /usr/local/etc/httpd/httpd.conf
            ...
            ServerName: www.gree2.com:8080

1. install mod_wsgi

    1. method 1 by pip

            $ pip install mod_wsgi
            ...

    1. verify installation

            $ mod_wsgi-express start-server
            Server URL         : http://localhost:8000/
            Server Root        : /tmp/mod_wsgi-localhost:8000:501
            Server Conf        : /tmp/mod_wsgi-localhost:8000:501/httpd.conf
            Error Log File     : /tmp/mod_wsgi-localhost:8000:501/error_log (warn)
            Request Capacity   : 5 (1 process * 5 threads)
            Request Timeout    : 60 (seconds)
            Startup Timeout    : 15 (seconds)
            Queue Backlog      : 100 (connections)
            Queue Timeout      : 45 (seconds)
            Server Capacity    : 20 (event/worker), 20 (prefork)
            Server Backlog     : 500 (connections)
            Locale Setting     : zh_CN.UTF-8

    1. run with wsgi.py

            $ mod_wsgi-express start-server wsgi.py
            ...

            $ open localhost:8000

    1. override port

            $ mod_wsgi-express start-server wsgi.py --port 8080

    1. find mod_wsgi.so from pip install

            $ mod_wsgi-express module-config
            LoadFile "/Users/gree2/anaconda2/envs/dev/lib/libpython2.7.dylib"
            LoadModule wsgi_module "/Users/gree2/anaconda2/envs/dev/lib/python2.7/site-packages/mod_wsgi/server/mod_wsgi-py27.so"
            WSGIPythonHome "/Users/gree2/anaconda2/envs/dev"

    1. config apache

            $ nano /usr/local/etc/httpd/httpd.conf
            ...
            LoadModule wsgi_module "/Users/gree2/anaconda2/envs/dev/lib/python2.7/site-packages/mod_wsgi/server/mod_wsgi-py27.so"
            ...

1. wsgi-scripts

    1. /usr/local/var/www/wsgi-scripts/

            $ cd /usr/local/var/www/
            $ mkdir wsgi-scripts
            $ nano demo.wsgi
            def application(environ, start_response):
                status = '200 OK'
                output = b'hello world'

                response_headers = [('Content-type', 'text/plain'),
                                    ('Content-Length', str(len(output)))]
                start_response(status, response_headers)
                return [output]

    1. use a flask app

            # flask app folder
            $ cd /usr/local/var/www/demo

            $ /usr/local/var/www/wsgi-scripts/
            $ nano demo.wsgi
            import sys
            sys.path.append('/usr/local/var/www/demo')
            from demo import manager as application

### reference

1. [github mod_wsgi](https://github.com/GrahamDumpleton/mod_wsgi)

1. [mod_wsgi configuration-guidelines](http://modwsgi.readthedocs.io/en/develop/user-guides/configuration-guidelines.html#)