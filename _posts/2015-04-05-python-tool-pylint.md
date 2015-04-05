---
layout: post
title: "python tool pylint"
description: ""
category: [python]
tags: [tool, pylint]
---
{% include JB/setup %}

### install

1. debian

        $ sudo apt-get install pylint

1. fedora

        $ sudo yum install pylint

1. opensuse

        $ sudo zypper install pylint # python2.7
        $ sudo zypper install python3-pylint

1. freebsd

        $ pip install pylint

1. arch linux

        $ pacman -S python2-pylint # if you live in the past
        $ pacman -S python-pylint  # if you line in the future

1. windows

        $ pip install pylint # see note

1. from source

        $ hg clone https://bitbucket.org/logilab/pylint/
        $ hg clone https://bitbucket.org/logilab/astroid
        $ mkdir logilab && touch logilab/__init__.py
        $ hg clone http://hg.logilab.org/logilab/common logilab/common
