---
layout: post
title: "docker on ubuntu"
description: ""
category: [docker]
tags: [installation, docker, ubuntu, run, usermod]
---
{% include JB/setup %}


### installation

1. steps

    1. check kernel version

            $ uname -r
            4.4.0-21-generic

    1. install via wget

            $ wget -qO- https://get.docker.com | sh
            [sudo] password for gree2:
            apparmor is enabled in the kernel and apparmor utils were already installed
            + sudo -E sh -c apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
            Executing: /tmp/tmp.LlNI1fJW0q/gpg.1.sh --keyserver
            hkp://ha.pool.sks-keyservers.net:80

            ...

            Client:
             Version:      1.11.1
             API version:  1.23
             Go version:   go1.5.4
             Git commit:   5604cbe
             Built:        Tue Apr 26 23:43:49 2016
             OS/Arch:      linux/amd64

            Server:
             Version:      1.11.1
             API version:  1.23
             Go version:   go1.5.4
             Git commit:   5604cbe
             Built:        Tue Apr 26 23:43:49 2016
             OS/Arch:      linux/amd64

            If you would like to use Docker as a non-root user, you should now consider
            adding your user to the "docker" group with something like:

              sudo usermod -aG docker gree2

            Remember that you will have to log out and back in for this to take effect!

    1. check

            $ sudo docker run hello-world
            Unable to find image 'hello-world:latest' locally
            latest: Pulling from library/hello-world
            4276590986f6: Pull complete 
            a3ed95caeb02: Pull complete 
            Digest: sha256:4f32210e234b4ad5cac92efacc0a3d602b02476c754f13d517e1ada048e5a8ba
            Status: Downloaded newer image for hello-world:latest

            Hello from Docker.
            This message shows that your installation appears to be working correctly.

            To generate this message, Docker took the following steps:
             1. The Docker client contacted the Docker daemon.
             2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
             3. The Docker daemon created a new container from that image which runs the
                executable that produces the output you are currently reading.
             4. The Docker daemon streamed that output to the Docker client, which sent it
                to your terminal.

            To try something more ambitious, you can run an Ubuntu container with:
             $ docker run -it ubuntu bash

            Share images, automate workflows, and more with a free Docker Hub account:
             https://hub.docker.com

            For more examples and ideas, visit:
             https://docs.docker.com/engine/userguide/

    1. ubuntu image

            $ sudo docker run -it ubuntu bash
            Unable to find image 'ubuntu:latest' locally
            latest: Pulling from library/ubuntu
            6d28225f8d96: Pull complete 
            166102ec41af: Pull complete 
            d09bfba2bd6a: Pull complete 
            c80dad39a6c0: Pull complete 
            a3ed95caeb02: Pull complete 
            Digest: sha256:5718d664299eb1db14d87db7bfa6945b28879a67b74f36da3e34f5914866b71c
            Status: Downloaded newer image for ubuntu:latest
            root@21d94b6f4b2a:/# uname -r
            4.4.0-21-generic
            root@21d94b6f4b2a:/# 

1. upgrade docker

    1. use -N option

            $ wget -N https://get.docker.com/ | sh

### reference

1. [installation](http://www.widuu.com/chinese_docker/installation/ubuntu.html)
