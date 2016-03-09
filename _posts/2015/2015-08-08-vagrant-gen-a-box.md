---
layout: post
title: "vagrant gen a box"
description: ""
category: [auto]
tags: [vagrant, vm, ubuntu, ssh]
---
{% include JB/setup %}


### base box and new project

1. choosing a base box

    1. ubuntu

            ubuntu/trusty64
            ubuntu/trusty32
            ubuntu/precise64
            ubuntu/precise32

    1. centos, debian, fedora, freebsd

            chef/centos-6.5
            chef/debian-7.4
            chef/fedora-20
            chef/freebsd-9.2

1. init a new project

            # host
            $ cd
            $ mkdir first-box-factory
            $ ^mkdir^cd

            # host
            $ vagrant init -m ubuntu/trusty64
            $ vagrant up

### installing some software

1. software

    1. [nodejs](https://nodejs.org/) check my post [2015 05 09 getting revealjs running on your computer]({% post_url 2015-05-09-getting-revealjs-running-on-your-computer %})

    1. [ruby](https://www.ruby-lang.org/en/) check my post [2015 03 08 setting up my mac]({% post_url 2015-03-08-setting-up-my-mac %})

    1. [jekyll](http://jekyllrb.com/) check my post [2015 03 04 recreate my github page]({% post_url 2015-03-04-recreate-my-github-page %})

    1. [lynx](http://lynx.isc.org/) text-based web browser [wikipedia](https://en.wikipedia.org/wiki/Lynx_(web_browser))

1. install

            # host
            $ vagrant ssh

            # guest
            $ sudo apt-get update -y
            $ sudo apt-get install nodejs -y
            $ sudo apt-get install lynx-cur -y
            $ sudo apt-get install ruby1.9.1-dev -y

            $ sudo gem install jekyll -y
            $ jekyll --version

### gen a box

1. gen

            $ vagrant package --output first-box-jekyll.box

### listing installing removing box

1. subcommands

            # host
            $ vagrant box list
            $ vagrant box add
            $ vagrant box remove

1. list boxes

            $ vagrant box list
            sparkmooc/base (virtualbox, 0)

            # box name      => sparkmooc/base
            # provider name => virtualbox
            # box version   => 0

            # host
            $ ls ~/.vagrant.d/boxes
            sparkmooc-VAGRANTSLASH-base

1. add box

            # host
            $ vagrant box add [name] [address]

            # address forms
            # 1. vendor/name
            # 2. http://...
            # 3. file:///...
            # 3. a local filename

            # host
            $ vagrant box add first-box-jekyll first-box-jekyll.box
            $ vagrant box list

1. remove box

            # host
            $ vagrant box remove [name]

            # host
            $ vagrant box remove [name] --provider [provider] --box-version [version]

            # host
            $ vagrant box remove first-box-jekyll

### using the box

1. find box

            # host
            $ vagrant box list | grep jekyll
            first-box-jekyll (virtualbox, 0)

1. create a blog project

            # host
            $ cd
            $ mkdir corporate-blog
            $ ^mkdir^cd

            # host
            $ vagrant init -m first-box-jekyll

            # host
            $ vagrant up
            $ vagrant ssh

            # guest
            $ cd /vagrant
            # gen the first version of the corporate blog
            $ jekyll new -f .

            # guest
            # build the html version
            $ jekyll build

            # guest
            $ jekyll serve -H 0.0.0.0 --detach
            ...
            Server address: http://0.0.0.0:4000/
            Server detached with pid '2159'. Run 'kill -9 2159' to stop the server.
            ...

            # guest
            $ lynx 127.0.0.1:4000

### forwarding ports

1. edit `Vagrantfile`

            # host
            $ pico Vagrantfile
            Vagrant.configure(2) do |config|
                config.vm.box = "first-box-jekyll"
                config.vm.network :forwarded_port, guest: 4000, host: 8080, host_ip: "127.0.0.1"
            end

1. apply the new changes

            # host
            $ vagrant reload

            # host
            $ vagrant ssh

            # guest
            $ cd /vagrant
            $ jekyll serve -H 0.0.0.0 --detach
