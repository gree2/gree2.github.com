---
layout: post
title: "running a notebook server"
description: ""
category: [python]
tags: [ipython, notebook, nvserver, profile, openssl]
---
{% include JB/setup %}


### [setup](http://ipython.org/ipython-doc/1/interactive/public_server.html)

1. [create a profile]({% post_url 2015-04-17-apache-spark-with-python-notebook-on-mac %})

1. notebook security

            In [1]: from IPython.lib import passwd
            In [2]: passwd()
            Enter password:
            Verify password:
            Out[2]: 'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'

1. edit `ipython_notebook_config.py`

            $ pico ~/.ipython/profile_spark/ipython_notebook_config.py
            # Password to use for web authentication
            c = get_config()
            c.NotebookApp.password =
            u'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'

1. use ssl

    1. start notebook to communicate via a secure protocol mode

            $ ipython notebook --profile=spark --certfile=mycert.pem

    1. gen self-signed certificate

            $ openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
            Generating a 1024 bit RSA private key
            ...++++++
            ....++++++
            writing new private key to 'mycert.pem'
            -----
            You are about to be asked to enter information that will be incorporated
            into your certificate request.
            What you are about to enter is what is called a Distinguished Name or a DN.
            There are quite a few fields but you can leave some blank
            For some fields there will be a default value,
            If you enter '.', the field will be left blank.
            -----
            Country Name (2 letter code) [AU]:cn
            State or Province Name (full name) [Some-State]:beijing
            Locality Name (eg, city) []:beijing
            Organization Name (eg, company) [Internet Widgits Pty Ltd]:icss
            Organizational Unit Name (eg, section) []:iig
            Common Name (e.g. server FQDN or YOUR name) []:node3
            Email Address []:hqlgree2@gmail.com

    1. config `ipython_notebook_config.py`

            $ pico ~/.ipython/profile_spark/ipython_notebook_config.py
            # Configuration file for ipython-notebook.

            c = get_config()
            # if you want plotting support always
            c.IPKernelApp.pylab = 'inline'
            c.NotebookApp.certfile = u'/home/hduser/bigdata/mycert.pem'
            c.NotebookApp.ip = '*'
            c.NotebookApp.open_browser = False
            c.NotebookApp.password = u'sha1:6063fb1b4a81:4862164516c871c1fb4d0fcbbaf37127462c5985'
            c.NotebookApp.port = 9999
