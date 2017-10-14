---
layout: post
title: "update ruby to latest version"
description: ""
category: [lang]
tags: [ruby, rvm]
---
{% include JB/setup %}


### steps

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
