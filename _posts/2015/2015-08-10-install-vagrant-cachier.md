---
layout: post
title: "install vagrant cachier"
description: ""
category: [auto]
tags: [vagrant, vagrant-cachier, ruby.taobao.org]
---
{% include JB/setup %}


### [vagrant-cachier](https://github.com/fgrehm/vagrant-cachier)

1. install

    1. with `--plugin-source`

            $ vagrant plugin install vagrant-cachier --plugin-source http://ruby.taobao.org
            Installing the 'vagrant-cachier' plugin. This can take a few minutes...
            Installed the plugin 'vagrant-cachier (1.2.1)'!


    1. tips from [github](https://github.com/sciurus/vagrant-mutate/issues/47)

1. install from source

    1. tips from [fgrehm.viewdocs.io](http://fgrehm.viewdocs.io/vagrant-cachier/development/)

            $ git clone https://github.com/fgrehm/vagrant-cachier.git
            $ cd vagrant-cachier
            $ bundle install
            $ bundle exec rake build
            $ vagrant plugin install pkg/vagrant-cachier-VERSION.gem

### usage

1. demo

    1. from [github](https://github.com/fgrehm/vagrant-cachier)

            Vagrant.configure("2") do |config|
              config.vm.box = 'your-box'
              if Vagrant.has_plugin?("vagrant-cachier")
                # Configure cached packages to be shared between instances of the same base box.
                # More info on http://fgrehm.viewdocs.io/vagrant-cachier/usage
                config.cache.scope = :box

                # OPTIONAL: If you are using VirtualBox, you might want to use that to enable
                # NFS for shared folders. This is also very useful for vagrant-libvirt if you
                # want bi-directional sync
                config.cache.synced_folder_opts = {
                  type: :nfs,
                  # The nolock option can be useful for an NFSv3 client that wants to avoid the
                  # NLM sideband protocol. Without this option, apt-get might hang if it tries
                  # to lock files needed for /var/cache/* operations. All of this can be avoided
                  # by using NFSv4 everywhere. Please note that the tcp option is not the default.
                  mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
                }
                # For more information please check http://docs.vagrantup.com/v2/synced-folders/basic_usage.html
              end
            end
