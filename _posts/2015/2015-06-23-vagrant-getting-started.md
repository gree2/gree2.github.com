---
layout: post
title: "vagrant getting started"
description: ""
category: [auto]
tags: [vagrant, virtualbox, up, ssh, destroy, networking, provisioning, teardown, rebuild]
---
{% include JB/setup %}


### info

1. Create and configure lightweight, reproducible, and portable development environments.

1. [download virtualbox](https://atlas.hashicorp.com/sparkmooc/boxes/base/versions/0.0.7.1/providers/virtualbox.box)

1. [download vagrant](http://www.vagrantup.com/downloads)

### getting started

1. up and running

            $ vagrant init hashicorp/precise32
            $ vagrant up

            # after running the above 2 commands
            # fully running vm in virtualbox
            # running ubuntu 12.04 lts 32-bit

            # ssh into this vm
            $ vagrant ssh
            
            # you're done playing around
            # remove all traces of it with
            $ vagrant destroy

1. project setup

    1. make the root directory of your project

            $ mkdir vagrant_getting_started

    1. describe the kind of machine and resources

            $ cd vagrant_getting_started

            # build-in command for initializing a directory
            # this will place `Vagrantfile` in current directory
            $ vagrant init

            # `Vagrantfile` is meant to be committed to version control with your project

1. boxes

    1. installing a box

            # if you haven't added a box yet
            $ vagrant box add hashicorp/precise32
            # this will download the box named `hashicorp/precise32`
            # from [HashiCorp's Atlas box catalog](https://atlas.hashicorp.com/boxes/search)

    1. using a box

            # configure our project
            $ pico Vagrantfile
            Vagrant.configure("2") do |config|
              config.vm.box = "hashicorp/precise32"
            end

    1. finding more boxes from [HashiCorp's Atlas box catalog](https://atlas.hashicorp.com/boxes/search)

1. up and ssh

    1. boot your first vagrant environment

            $ vagrant up

    1. ssh into the machine

            # since vagrant runs vm without a ui
            $ vagrant ssh

            # vagrant shares a directory at /vagrant
            # with the directory on the host containing `Vagrantfile`

            # when done
            $ vagrant destroy
            # back on your host machine
            # vagrant will remove all traces of the vm

1. synced folders

    1. vagrant will automatically sync files to and from guest machine

    1. by default vagrant shares your project directory

            $ vagrant up
            ...
            $ vagrant ssh
            ...
            vagrant@precise32:~$ ls /vagrant
            Vagrantfile

1. provisioning

    1. installing apache

            # in the same directory of `Vagrantfile`
            $ pico bootstrap.sh
            #!/usr/bin/env bash

            apt-get update
            apt-get install -y apache2
            if ! [ -L /var/www ]; then
              rm -rf /var/www
              ln -fs /vagrant /var/www
            fi

            # next configure vagrant to run this script
            $ pico Vagrantfile
            Vagrant.configure("2") do |config|
              config.vm.box = "hashicorp/precise32"
              config.vm.provision :shell, path: "bootstrap.sh"
            end

            # the `provision` line tells vagrant
            # to use the `shell` provisioner to setup the machine

    1. provision

            # after everything is configured just run
            $ vagrant up
            # this will create your machine
            # and vagrant will automatically provision it

            # if vm is running from a previous step
            $ vagrant reload --provision
            # will restarted vm
            # skipping initial import step

            # after vagrant completes running
            # web server will be up and running
            $ vagrant ssh
            ...
            vagrant@precise32:~$ wget -qO- 127.0.0.1

1. networking

    1. port forwarding

            $ pico Vagrantfile
            Vagrant.configure("2") do |config|
              config.vm.box = "hashicorp/precise32"
              config.vm.provision :shell, path: "bootstrap.sh"
              config.vm.network :forwarding_port, host: 4567, guest: 80
            end

            # run
            $ vagrant reload
            # or (depending on if the vm is already running)
            $ vagrant up

            # once the machine is running again
            # load `http://127.0.0.1:4567` in your browser

    1. other networking

            # 1. assign static ip address
            # 2. bridge the guest machine onto an existing network

1. share called [vagrant share](https://docs.vagrantup.com/v2/share/)

    1. login to [hashicorp's atlas](https://atlas.hashicorp.com/)

            # once you have an account
            # log in using
            $ vagrant login
            Username or Email: hqlgree2
            Password (will be hidden):
            You're now logged in!

    1. share

            # once logged in
            $ vagrant share
            ...
            ==> default: Your Vagrant Share is running!
            ==> default: URL: http://frosty-weasel-0857.vagrantshare.com
            ...

            # 1. copy the url outputted for you
            # 2. visit that in a web browser
            # 3. modify your `index.html` and refresh the url. if will be updated
            # 4. to end the sharing session
            # hit `ctrl+c`

1. teardown

    1. suspending

            $ vagrant suspend
            # save the current running state of the machine and stop it

            $ vagrant up
            # will be resumed from where you left off

    1. halting

            $ vagrant halt
            # gracefully shut down guest os and power down

            $ vagrant up
            # boot it again

    1. destroying

            $ vagrant destroy
            # remove all traces of the guest machine from your system
            # 1. stop the guest machine
            # 2. power it down
            # 3. remove all of the guest hard disks

1. rebuild

            $ vagrant up
            # run this at any time
            # vagrant will recreate your work environment

1. providers

    1. your project was always backed with [virtualbox](http://www.virtualbox.org/)

    1. but vagrant can work with a wide variety of backend providers, such as [vmware](https://docs.vagrantup.com/v2/vmware/), [aws](http://github.com/mitchellh/vagrant-aws)

            $ vagrant up --provider=vmware_fusion
            ...

            $ vagrant up --provider=aws

### demo

1. `pre-download` or copy from the box of your partner [reference](http://tuanchauict.blogspot.com/2014/09/vagrant-development-environments-made.html)

            $ pico Vagrantfile
            ...
              config.vm.box_url="path/to/box/file.box"
            ...

            # it will help you save time downloading

1. my demo 

    1. Vagrantfile

            $ pico Vagrantfile
            # -*- mode: ruby -*-
            # vi: set ft=ruby :

            # Ipython port to forward (also set in IPython notebook config)
            ipythonPort = 8001

            Vagrant.configure(2) do |config|
              config.ssh.insert_key = true
              config.vm.define "sparkvm" do |master|
                master.vm.box = "sparkmooc/base"
                master.vm.box_url = "/Users/hqlgree2/.vagrant.d/boxes/virtualbox.box"
                master.vm.box_download_insecure = true
                master.vm.boot_timeout = 900
                # IPython port (set in notebook config)
                master.vm.network :forwarded_port, host: ipythonPort, guest: ipythonPort, auto_correct: true
                # Spark UI (Driver)
                master.vm.network :forwarded_port, host: 4040, guest: 4040, auto_correct: true
                master.vm.hostname = "sparkvm"
                master.vm.usable_port_range = 4040..4090

                master.vm.provider :virtualbox do |v|
                  v.name = master.vm.hostname.to_s
                end
              end
            end

    1. start

            $ vagrant up
            Bringing machine 'sparkvm' up with 'virtualbox' provider...
            ==> sparkvm: Box 'sparkmooc/base' could not be found. Attempting to find and install...
                sparkvm: Box Provider: virtualbox
                sparkvm: Box Version: >= 0
            ==> sparkvm: Adding box 'sparkmooc/base' (v0) for provider: virtualbox
                sparkvm: Downloading: file:///Users/hqlgree2/.vagrant.d/boxes/virtualbox.box
            ==> sparkvm: Successfully added box 'sparkmooc/base' (v0) for 'virtualbox'!
            ==> sparkvm: Importing base box 'sparkmooc/base'...
            ==> sparkvm: Matching MAC address for NAT networking...
            ==> sparkvm: Setting the name of the VM: sparkvm
            ==> sparkvm: Clearing any previously set network interfaces...
            ==> sparkvm: Preparing network interfaces based on configuration...
                sparkvm: Adapter 1: nat
            ==> sparkvm: Forwarding ports...
                sparkvm: 8001 => 8001 (adapter 1)
                sparkvm: 4040 => 4040 (adapter 1)
                sparkvm: 22 => 2222 (adapter 1)
            ==> sparkvm: Booting VM...
            ==> sparkvm: Waiting for machine to boot. This may take a few minutes...
                sparkvm: SSH address: 127.0.0.1:2222
                sparkvm: SSH username: vagrant
                sparkvm: SSH auth method: private key
                sparkvm: Warning: Connection timeout. Retrying...
                sparkvm: 
                sparkvm: Vagrant insecure key detected. Vagrant will automatically replace
                sparkvm: this with a newly generated keypair for better security.
                sparkvm: 
                sparkvm: Inserting generated public key within guest...
                sparkvm: Removing insecure key from the guest if its present...
                sparkvm: Key inserted! Disconnecting and reconnecting using new SSH key...
            ==> sparkvm: Machine booted and ready!
            ==> sparkvm: Checking for guest additions in VM...
            ==> sparkvm: Setting hostname...
            ==> sparkvm: Mounting shared folders...
                sparkvm: /vagrant => /Users/hqlgree2/Documents/hadoop/vagrant
