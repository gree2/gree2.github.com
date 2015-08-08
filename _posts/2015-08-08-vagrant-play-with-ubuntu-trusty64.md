---
layout: post
title: "vagrant play with ubuntu trusty64"
description: ""
category: [auto]
tags: [vagrant, ubuntu, trusty64, sed]
---
{% include JB/setup %}


### day 0

1. create `Vagrantfile`

            # host
            $ vagrant box list
            sparkmooc/base  (virtualbox, 0)
            ubuntu/trusty64 (virtualbox, 0)

            # host
            $ vagrant init -m ubuntu/trusty64

1. shell provisioning

            # host
            $ pico script.sh
            #!/usr/bin/env bash
            sudo sed 's@archive.ubuntu.com@mirrors.163.com@' -i /etc/apt/sources.list
            sudo sed 's@archive.canonical.com@mirrors.163.com@' -i /etc/apt/sources.list
            sudo sed 's@security.ubuntu.com@mirrors.163.com@' -i /etc/apt/sources.list
            sudo apt-get update

1. edit `Vagrantfile` add `config.vm.provision`

            # host
            $ pico Vagrantfile
            Vagrant.configure(2) do |config|
              config.vm.box = "ubuntu/trusty64"
              config.vm.provision "shell", path: "script.sh"
            end

1. start up vm

            # host
            $ vagrant up
