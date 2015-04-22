---
layout: post
title: "docker on mac"
description: ""
category: [docker]
tags: [boot2docker, docker]
---
{% include JB/setup %}


### install boot2docker

1. go to [boot2docker/osx-installer](https://github.com/boot2docker/osx-installer/releases/latest)

1. download `Boot2Docker-x.x.x.pkg`

1. install boot2docker by double-clicking the package

    * the installer places `Boot2Docker` in your `Application` folder

    * the installer places `docker` and `boot2docker` in `/usr/local/bin`

### start the boot2docker application

1. to run a docker container, you first start the `boot2docker` vm

1. then issue `docker` commands to `create, load and manage` containers

1. you can launch `boot2docker` from application folder or form command line

### from the application folder

* opens a terminal window

* creates a ~/.boot2docker dir

* creates a virtualbox iso and certs

* start a virtualbox vm running the docker daemon

    1. `$ docker run hello-world`

            $ docker run hello-world
            Unable to find image 'hello-world:latest' locally
            511136ea3c5a: Pull complete
            31cbccb51277: Pull complete
            e45a5af57b00: Pull complete
            hello-world:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
            Status: Downloaded newer image for hello-world:latest
            Hello from Docker.
            This message shows that your installation appears to be working correctly.

            To generate this message, Docker took the following steps:
             1. The Docker client contacted the Docker daemon.
             2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
                (Assuming it was not already locally available.)
             3. The Docker daemon created a new container from that image which runs the
                executable that produces the output you are currently reading.
             4. The Docker daemon streamed that output to the Docker client, which sent it
                to your terminal.

    1. `$ docker run -it ubuntu bash`

            $ docker run -it ubuntu bash
            Unable to find image 'ubuntu:latest' locally
            latest: Pulling from ubuntu
            706766fe1019: Pull complete 
            a62a42e77c9c: Pull complete 
            2c014f14d3d9: Pull complete 
            b7cf8f0d9e82: Already exists 
            ubuntu:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
            Digest: sha256:80d996c3693d792505039a3612cf9221ae9704317ea11fda75c86ede672ee1b3
            Status: Downloaded newer image for ubuntu:latest

### from command line

1. create a new `boot2docker` vm

            $ boot2docker init
            # this creates a new vm
            # you only need to run this command once

1. start the `boot2docker` vm

            $ boot2docker start

1. display the environment variables for the docker client

            $ boot2docker shellinit
            Writing /Users/hqlgree2/.boot2docker/certs/boot2docker-vm/ca.pem
            Writing /Users/hqlgree2/.boot2docker/certs/boot2docker-vm/cert.pem
            Writing /Users/hqlgree2/.boot2docker/certs/boot2docker-vm/key.pem
                export DOCKER_CERT_PATH=/Users/hqlgree2/.boot2docker/certs/boot2docker-vm
                export DOCKER_TLS_VERIFY=1
                export DOCKER_HOST=tcp://192.168.59.103:2376

1. to set the environment variables in your shell do the following

            $ eval "$(boot2docker shellinit)"
            
    * you can also set them manually by using the `export` command `boot2docker` returns

1. run the `hello-world` container to verify your setup

            $ docker run hello-world

### upgrade boot2docker

1. open a terminal on your local machine

1. stop the `boot2docker` application

            $ boot2docker stop

1. run the upgrade command

            $ boot2docker upgrade

### access container ports

1. start an nginx container on the DOCKER_HOST

            $ docker run -d -P --name web nginx

    1. the `docker run` commands starts a container runs it and then exits

    1. the `-d` flag keeps the container running in the background after the `docker run` command completes

    1. the `-P` flag publishes exposed ports from the container to your local host

1. display your running container with `docker ps` command

            $ docker ps

    * at this point you can see `nginx` is running as a daemon

1. view just the container's ports

            $ docker port web
            443/tcp -> 0.0.0.0:49156
            80/tcp -> 0.0.0.0:49157

    * `web` container's port `80` is mapped to port `49157` on your docker host

1. enter `http://localhost:49157` address (`localhost` is `0.0.0.0`) in your browser

            this webpage is not available

    * this didn't work

    * `DOCKER_HOST` address is not the localhost address `0.0.0.0`

    * but instead the address of the `boot2docker` vm

1. get the address of the `boot2docker` vm

            $ boot2docker ip
            192.168.59.103

1. enter the `http://192.168.59.103:49157`

            welcome to nginx!
            ...

1. to stop and then remove your running `nginx` container

            $ docker stop web
            $ docker rm web

### mount a volume on the container

* when you start `boot2docker` it automatically shares your `/Users` directory with the vm

1. change to your user `$HOME` directory

            $ cd

1. make a new `site` directory

            $ mkdir site

1. change into the `site` directory

            $ cd site

1. create a new `index.html` file

            $ echo "my new site" > index.html

1. start a new `nginx` container and replace the `html` folder with your `site` directory

            $ docker run -d -P -v $HOME/site:/usr/share/nginx/html --name mysite nginx

1. get the `mysite` container's port

            $ docker port mysite
            80/tcp -> 0.0.0.0:49166
            443/tcp -> 0.0.0.0:49165

1. open the site in a browser `http://192.168.59.103:49166`

1. try adding a page to your `$HOME/site` in real time

            $ echo "this is cool" > cool.html

1. open the new page in the browser `http://192.168.59.103:49166/cool.html`

1. stop and then remove your running `mysite` container

            $ docker stop mysite
            $ docker rm mysite