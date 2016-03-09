---
layout: post
title: "docker getting started with docker compose"
description: ""
category: [docker]
tags: [docker, compose]
---
{% include JB/setup %}


### info

1. compose is a tool for defining and running complex apps with docker

1. [docs](http://docs.docker.com/compose/)

    1. define app's environment with a `Dockerfile`

            FROM python:2.7
            WORKDIR /code
            ADD requirements.txt /code/
            RUN pip install -r requirements.txt
            ADD . /code
            CMD python app.py

    1. define services that make up your app in `docker-compose.yml`

            web:
              build: .
              links:
               - db
              port:
               - "8000:8000"
            db:
              image: postgres

    1. run `docker-compose up` and compose will start and run your entire app

### quick start

1. install compose

    1. use commands

            $ curl -L https://github.com/docker/compose/releases/download/1.2.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
            $ chmod +x /usr/local/bin/docker-compose

    1. installed as a python package

            $ sudo pip install -U docker-compose

            $ docker-compose --version

1. make a directory for the project

            $ mkdir composetest
            $ cd composetest

1. create a simple web app use `flask framework`

            $ pico app.py
            from flask import Flask
            from redis import Redis
            import os
            app = Flask(__name__)
            redis = Redis(host='redis', port=6379)

            @app.route('/')
            def hello():
                redis.incr('hits')
                return 'hello world! I have been seen %s times.' % redis.get('hits')

            if __name__ == '__main__'
                app.run(host='0.0.0.0', debug=True)

1. define python dependencies in `requirements.txt`

            $ pico requirements.txt
            flask
            redis

1. create a docker image

    * specify how to build the image using `Dockerfile`

            $ pico Dockerfile
            FROM python:2.7
            ADD . /code
            WORKDIR /code
            RUN pip install -r requirements.txt

1. define services

    * define a set of services using `docker-compose.yml`

            $ pico docker-compose.yml
            web:
              build: .
              command: python app.py
              ports:
               - '5000:5000'
              volumes:
               - .:/code
              links:
               - redis
            redis:
              image: redis

    * this define two services `web` and [redis](https://registry.hub.docker.com/_/redis/)

            web:
                1. which is built from the Dockerfile
                2. run command python app.py inside the image
                3. forword exposed port 5000 on the container
                4. to port 5000 on the host machine
                5. connect up the redis service
                6. mount the current directory inside the container

            redis:
                which uses the public image redis

1. build and run your app with compose

            $ docker-compose up

            Creating dockercompose_redis_1...
            Pulling image redis:latest...
            latest: Pulling from redis
            7a3e804ed6c0: Download complete
            b96d1548a24e: Download complete
            5ba9a5b9710f: Download complete
            37f07aacbfe5: Download complete
            ec7f3a6b5dc6: Download complete
            499b313c4d4e: Download complete
            e3c38490a53a: Download complete
            b508080bc4db: Download complete
            b5edc072cfec: Download complete
            41eb1212d9f4: Download complete
            2c010358721d: Download complete
            fb83dcd979bd: Download complete
            a1811b7b024f: Download complete
            05a396cb49e2: Download complete
            361283d1af1a: Download complete
            de77586468a2: Download complete
            0f3059144681: Download complete
            Status: Downloaded newer image for redis:latest
            Creating dockercompose_web_1...
            Building web...
            Step 0 : FROM python:2.7
            2.7: Pulling from python
            c9e3effdd23a: Pull complete
            24b3549417de: Pull complete
            3009dde7a8a5: Pull complete
            9da97b9166df: Pull complete
            a80d186674da: Pull complete
            c700c43ccd75: Pull complete
            26c01397e7df: Pull complete
            2abef589f374: Pull complete
            93d77aec17a0: Already exists
            39bb80489af7: Already exists
            df2a0347c9d0: Already exists
            7a3871ba15f8: Already exists
            a2703ed272d7: Already exists
            python:2.7: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
            Digest: sha256:f8398c96bc62ee30fab45d44f8e1db2161b9a8e1f83e0fb0ae71b96950a3b329
            Status: Downloaded newer image for python:2.7
             ---> 93d77aec17a0
            Step 1 : ADD . /code
             ---> 9ee91686033a
            Removing intermediate container e61556a8e1e6
            Step 2 : WORKDIR /code
             ---> Running in e93e5c940b95
             ---> 9ee162f2324e
            Removing intermediate container e93e5c940b95
            Step 3 : RUN pip install -r requirements.txt
             ---> Running in 5a858f13c524
            Collecting flask (from -r requirements.txt (line 1))
              Downloading Flask-0.10.1.tar.gz (544kB)
            Collecting redis (from -r requirements.txt (line 2))
              Downloading redis-2.10.3.tar.gz (86kB)
            Collecting Werkzeug>=0.7 (from flask->-r requirements.txt (line 1))
              Downloading Werkzeug-0.10.4-py2.py3-none-any.whl (293kB)
            Collecting Jinja2>=2.4 (from flask->-r requirements.txt (line 1))
              Downloading Jinja2-2.7.3.tar.gz (378kB)
            Collecting itsdangerous>=0.21 (from flask->-r requirements.txt (line 1))
              Downloading itsdangerous-0.24.tar.gz (46kB)
            Collecting markupsafe (from Jinja2>=2.4->flask->-r requirements.txt (line 1))
              Downloading MarkupSafe-0.23.tar.gz
            Building wheels for collected packages: flask, redis, Jinja2, itsdangerous, markupsafe
              Running setup.py bdist_wheel for flask
              Stored in directory: /root/.cache/pip/wheels/d2/db/61/cb9b80526b8f3ba89248ec0a29d6da1bb6013681c930fca987
              Running setup.py bdist_wheel for redis
              Stored in directory: /root/.cache/pip/wheels/b4/82/09/7713ff9bc6f6a58e41b0a89433aed5d2293fd5868f76adef94
              Running setup.py bdist_wheel for Jinja2
              Stored in directory: /root/.cache/pip/wheels/b3/c0/62/8908d15b90a7de8bd15e2e6e5f5ba1398c0d9940d62a6bf8f5
              Running setup.py bdist_wheel for itsdangerous
              Stored in directory: /root/.cache/pip/wheels/97/c0/b8/b37c320ff57e15f993ba0ac98013eee778920b4a7b3ebae3cf
              Running setup.py bdist_wheel for markupsafe
              Stored in directory: /root/.cache/pip/wheels/94/a7/79/f79a998b64c1281cb99fa9bbd33cfc9b8b5775f438218d17a7
            Successfully built flask redis Jinja2 itsdangerous markupsafe
            Installing collected packages: Werkzeug, markupsafe, Jinja2, itsdangerous, flask, redis
            Successfully installed Jinja2-2.7.3 Werkzeug-0.10.4 flask-0.10.1 itsdangerous-0.24 markupsafe-0.23 redis-2.10.3
             ---> 39e8424b8205
            Removing intermediate container 5a858f13c524
            Successfully built 39e8424b8205
            Attaching to dockercompose_redis_1, dockercompose_web_1
            redis_1 | 1:C 09 Jun 02:26:47.968 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
            redis_1 |                 _._                                                  
            redis_1 |            _.-``__ ''-._                                             
            redis_1 |       _.-``    `.  `_.  ''-._           Redis 3.0.2 (00000000/0) 64 bit
            redis_1 |   .-`` .-```.  ```\/    _.,_ ''-._                                   
            redis_1 |  (    '      ,       .-`  | `,    )     Running in standalone mode
            redis_1 |  |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
            redis_1 |  |    `-._   `._    /     _.-'    |     PID: 1
            redis_1 |   `-._    `-._  `-./  _.-'    _.-'                                   
            redis_1 |  |`-._`-._    `-.__.-'    _.-'_.-'|                                  
            redis_1 |  |    `-._`-._        _.-'_.-'    |           http://redis.io        
            redis_1 |   `-._    `-._`-.__.-'_.-'    _.-'                                   
            redis_1 |  |`-._`-._    `-.__.-'    _.-'_.-'|                                  
            redis_1 |  |    `-._`-._        _.-'_.-'    |                                  
            redis_1 |   `-._    `-._`-.__.-'_.-'    _.-'                                   
            redis_1 |       `-._    `-.__.-'    _.-'                                       
            redis_1 |           `-._        _.-'                                           
            redis_1 |               `-.__.-'                                               
            redis_1 | 
            redis_1 | 1:M 09 Jun 02:26:47.969 # Server started, Redis version 3.0.2
            redis_1 | 1:M 09 Jun 02:26:47.969 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
            redis_1 | 1:M 09 Jun 02:26:47.969 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
            redis_1 | 1:M 09 Jun 02:26:47.969 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
            redis_1 | 1:M 09 Jun 02:26:47.970 * DB loaded from disk: 0.000 seconds
            redis_1 | 1:M 09 Jun 02:26:47.970 * The server is now ready to accept connections on port 6379
            web_1   |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
            web_1   |  * Restarting with stat
            web_1   | 192.168.59.3 - - [09/Jun/2015 02:27:38] "GET / HTTP/1.1" 200 -


            # if using boot2docker
            $ boot2docker ip
            192.168.59.103
            # will tell you its address

    * run your services in the background

            # -d flag for daemon mode
            $ docker-compose up -d

            # to see what is currently running
            $ docker-compose ps

            # run one-off commands for your services
            # to see what environment variables
            # are available to the web service
            $ docker-compose run web env

    * stop your services

            $ docker-compose stop