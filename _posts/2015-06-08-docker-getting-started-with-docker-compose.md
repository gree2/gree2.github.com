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

            # if using boot2docker
            $ boot2docker ip
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