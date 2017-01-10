---
layout: post
title: "coreos up and running"
description: ""
category: [auto, linux]
tags: [virtualbox, vagrant, git, coreos]
---
{% include JB/setup %}


### steps - coreos up

1. on windows

    1. download [virtualbox](https://www.virtualbox.org) from [https://www.virtualbox.org](https://www.virtualbox.org/wiki/Downloads) and install it

    1. download [vagrant_1.8.3.msi](https://www.vagrantup.com) from [www.vagrantup.com](https://www.vagrantup.com/downloads.html) and install it

    1. clone [coreos-vagrant](https://github.com/coreos/coreos-vagrant/) github project

            # 1. clone
            $ git clone https://github.com/coreos/coreos-vagrant/

            # 2. cloud-config
            $ cd coreos-vagrant
            $ mv user-data.sample user-data

            # 3. replace text between `etcd2` and `fleet`
            etcd2:
              name: core-01
              initial-advertise-peer-urls: http://$private_ipv4:2380
              listen-peer-urls: http://$private_ipv4:2380,http://$private_ipv4:7001
              initial-cluster-token: core-01_etcd
              initial-cluster: core-01=http://$private_ipv4:2380
              initial-cluster-state: new
              advertise-client-urls: http://$private_ipv4:2379, http://$public_ipv4:4001
              listen-client-urls: http://0.0.0.0:2379,http://0.0.0.0:4001
            fleet:

    1. startup and ssh

            # 1. boot coreos vm host
            $ vagrant up

            # 2. open `ssh` connection to vm
            $ vagrant ssh

            # 3. verify `etcd fleet docker`
            $ systemctl status etcd2
            $ systemctl status fleet
            $ docker version

1. on mac