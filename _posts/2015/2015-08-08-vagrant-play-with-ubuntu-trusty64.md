---
layout: post
title: "vagrant play with ubuntu trusty64"
description: ""
category: [auto]
tags: [vagrant, ubuntu, trusty64, sed]
---
{% include JB/setup %}


### day 0 shell provisioning

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

### day 1 gen a box using shell provisioning

1. script.sh

            $ pico script.sh
            #!/usr/bin/env bash

            echo "replace sources.list => mirrors.163.com"
            sudo sed 's@archive.ubuntu.com@mirrors.163.com@' -i /etc/apt/sources.list
            sudo sed 's@archive.canonical.com@mirrors.163.com@' -i /etc/apt/sources.list
            sudo sed 's@security.ubuntu.com@mirrors.163.com@' -i /etc/apt/sources.list
            echo "install packages     => nodejs lynx-cur ruby1.9.1-dev"
            sudo apt-get update -y                #>>/home/vagrant/provision-script.log 2>&1
            sudo apt-get install nodejs -y        #>>/home/vagrant/provision-script.log 2>&1
            sudo apt-get install lynx-cur -y      #>>/home/vagrant/provision-script.log 2>&1
            sudo apt-get install ruby1.9.1-dev -y #>>/home/vagrant/provision-script.log 2>&1
            echo "replace gem sources  => http://ruby.taobao.org/"
            sudo gem sources --remove https://rubygems.org/ #>>/home/vagrant/provision-script.log 2>&1
            sudo gem sources -a http://ruby.taobao.org/     #>>/home/vagrant/provision-script.log 2>&1
            sudo gem sources -l                             #>>/home/vagrant/provision-script.log 2>&1
            echo "install bundler jekyll"
            sudo gem install bundler #>>/home/vagrant/provision-script.log 2>&1
            sudo gem install jekyll  #>>/home/vagrant/provision-script.log 2>&1
            jekyll --version         #>>/home/vagrant/provision-script.log 2>&1

1. gen a box

            # host
            $ cd vagrant-jekyll-shell
            $ vagrant up
            $ vagrant package --output vagrant-jekyll-shell-v0.1.0.box
            ==> default: Attempting graceful shutdown of VM...
            ==> default: Clearing any previously set forwarded ports...
            ==> default: Exporting VM...
            ==> default: Compressing package to: /Users/hqlgree2/Documents/github/ansible/vagrant/vagrant-jekyll-shell/vagrant-jekyll-shell-v0.1.0.box

1. using the shell-provisioned box

    1. add

            # host
            $ vagrant box add vagrant-jekyll-shell-v0.1.0 vagrant-jekyll-shell-v0.1.0.box
            ==> box: Adding box 'vagrant-jekyll-shell-v0.1.0' (v0) for provider: 
                box: Downloading: file:///Users/hqlgree2/Documents/github/ansible/vagrant/vagrant-jekyll-shell/vagrant-jekyll-shell-v0.1.0.box
            ==> box: Successfully added box 'vagrant-jekyll-shell-v0.1.0' (v0) for 'virtualbox'!

    1. list

            # host
            $ vagrant box list
            sparkmooc/base              (virtualbox, 0)
            ubuntu/trusty64             (virtualbox, 0)
            vagrant-jekyll-shell-v0.1.0 (virtualbox, 0)

    1. using

            # host
            $ mkdir flowers-vagrant-jekyll-shell
            $ vagrant init -m vagrant-jekyll-shell-v0.1.0

            # host
            $ pico Vagrantfile
            Vagrant.configure(2) do |config|
                config.vm.box = "vagrant-jekyll-shell-v0.1.0"
                config.vm.network :forwarded_port, guest: 4000, host: 8100, host_ip: "127.0.0.1"

            $script = <<SCRIPT

            cd /vagrant
            jekyll serve -H 0.0.0.0 --detach

            SCRIPT

                config.vm.provision "shell", inline: $script, run: "always"

            end

            # host
            $ vagrant up

            # guest
            $ cd /vagrant
            $ jekyll new -f .
            $ jekyll build
            $ jekyll serve -H 0.0.0.0 --detach
            $ logout

1. jekyll

    1. new

            # guest
            $ jekyll new -f .

            # host
            $ tree
            .
            ├── Vagrantfile
            ├── _config.yml
            ├── _includes
            │   ├── footer.html
            │   ├── head.html
            │   └── header.html
            ├── _layouts
            │   ├── default.html
            │   ├── page.html
            │   └── post.html
            ├── _posts
            │   └── 2015-08-09-welcome-to-jekyll.markdown
            ├── _sass
            │   ├── _base.scss
            │   ├── _layout.scss
            │   └── _syntax-highlighting.scss
            ├── about.md
            ├── css
            │   └── main.scss
            ├── feed.xml
            └── index.html

    1. build

            # guest
            $ jekyll build
            Configuration file: /vagrant/_config.yml
                        Source: /vagrant
                   Destination: /vagrant/_site
                  Generating... 
                                done.
             Auto-regeneration: disabled. Use --watch to enable.

            # host
            $ tree
            .
            ├── Vagrantfile
            ├── _config.yml
            ├── _includes
            │   ├── footer.html
            │   ├── head.html
            │   └── header.html
            ├── _layouts
            │   ├── default.html
            │   ├── page.html
            │   └── post.html
            ├── _posts
            │   └── 2015-08-09-welcome-to-jekyll.markdown
            ├── _sass
            │   ├── _base.scss
            │   ├── _layout.scss
            │   └── _syntax-highlighting.scss
            ├── _site
            │   ├── Vagrantfile
            │   ├── about
            │   │   └── index.html
            │   ├── css
            │   │   └── main.css
            │   ├── feed.xml
            │   ├── index.html
            │   └── jekyll
            │       └── update
            │           └── 2015
            │               └── 08
            │                   └── 09
            │                       └── welcome-to-jekyll.html
            ├── about.md
            ├── css
            │   └── main.scss
            ├── feed.xml
            └── index.html

    1. serve

            # guest
            $ jekyll serve -H 0.0.0.0 --detach
            Configuration file: /vagrant/_config.yml
                        Source: /vagrant
                   Destination: /vagrant/_site
                  Generating... 
                                done.
             Auto-regeneration: enabled for '/vagrant'
            Configuration file: /vagrant/_config.yml
                Server address: http://0.0.0.0:4000/
            Server detached with pid '1528'. Run `kill -9 1528' to stop the server.
