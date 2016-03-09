---
layout: post
title: "setup mongodb on ubuntu 15.04"
description: ""
category: [nosql]
tags: [ubuntu, 15.04, upstart-sysv, systemd]
---
{% include JB/setup %}


### [setup](http://www.answee.com/8a5a9f9b/mongodb-2-6-does-not-start-on-ubuntu-15-04)

1. `systemd` to `upstart-sysv`

            # ubuntu 15.04 advanced the default init system
            # from `upstart` to systemd

            # switch back to `upstart`
            $ sudo apt-get install upstart-sysv
            $ sudo reboot

1. install

            # 1. import public key
            $ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10
            # 2. create a list file for mongodb
            $ sudo echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
            # 3. reload local package database
            $ sudo apt-get update
            # 4. install
            sudo('apt-get install -y mongodb-org')

1. service

            $ sudo service mongod start

1. [uninstall](http://askubuntu.com/questions/617097/mongodb-2-6-does-not-start-on-ubuntu-15-04)

            # 1. stop mongodb
            $ sudo service mongod stop
            # 2. remove package
            $ sudo apt-get purge mongodb-org*
            # 3. remove data directories
            $ sudo rm -r /var/log/mongodb
            $ sudo rm -r /var/lib/mongodb

### references

1. [support for ubuntu 15.04](https://jira.mongodb.org/browse/SERVER-17742)

1. [install mongodb on ubuntu](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/)