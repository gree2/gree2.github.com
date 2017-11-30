---
layout: post
title: "update ruby to latest version"
description: ""
category: [lang]
tags: [ruby, rvm]
---
{% include JB/setup %}


### steps for mac

1. install rvm

    1. install

            $ curl -L get.rvm.io | bash -s stable
            ...
            Upgrade Notes:
            * WARNING: '~/.profile' file found. To load it into your shell, add the following line to '/Users/gree2/.bash_profile':
              source ~/.profile
            * No new notes to display.

    1. check install

            $ source ~/.profile
            $ source ~/.bash_profile

            $ rvm -v

1. update ruby

    1. check ruby version

            $ rvm -v
            rvm 1.29.3 (latest) by Michal Papis, Piotr Kuczynski, Wayne E. Seguin [https://rvm.io]

    1. list ruby versions

            $ rvm list known
            # MRI Rubies
            ...
            [ruby-]2.2[.7]
            [ruby-]2.3[.4]
            [ruby-]2.4[.1]
            ...

    1. install ruby

            $ rvm install 2.4.1

    1. check ruby version

            $ ruby -v
            ruby 2.4.1p111 (2017-03-22 revision 58053) [x86_64-darwin16]

### steps for centos

1. install rvm

    1. add keys

        $ gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3

    1. install rvm

        $ curl -L get.rvm.io | bash -s stable

        Downloading https://github.com/rvm/rvm/archive/1.29.3.tar.gz
        Downloading https://github.com/rvm/rvm/releases/download/1.29.3/1.29.3.tar.gz.asc
        gpg: ...
        ...
        GPG verified '/usr/local/rvm/archives/rvm-1.29.3.tgz'
        Creating group 'rvm'

        Installing RVM to /usr/local/rvm/
        Installation of RVM in /usr/local/rvm/ is almost complete:

          * First you need to add all users that will be using rvm to 'rvm' group,
            and logout - login again, anyone using rvm will be operating with `umask u=rwx,g=rwx,o=rx`.

          * To start using RVM you need to run `source /etc/profile.d/rvm.sh`
            in all your open shell windows, in rare cases you need to reopen all shell windows.

1. source and install

    1. source

        $ source /etc/profile.d/rvm.sh
        $ rvm list known
        $ rvm install 2.3.4

    1. change sources and install gems

        $ gem sources -l
        $ gem sources -r https://rubygems.org/
        $ gem sources --add https://gems.ruby-china.org/
        $ gem install bundler
        $ gem install rails
        $ gem install jekyll
