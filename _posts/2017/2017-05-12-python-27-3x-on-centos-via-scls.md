---
layout: post
title: "python 2.7 3.x on centos via scls"
description: ""
category: [linux, python]
tags: [scl, centos, scl, rh-python, bash, list]
---
{% include JB/setup %}


### steps

1. [quick start](https://lists.centos.org/pipermail/centos-announce/2015-December/021555.html)

    1. install python 3.5

            $ sudo yum install centos-release-scl
            $ sudo yum install rh-python35
            $ scl enable rh-python35 bash

    1. use python

            $ python
            Python 3.5.1 (default, Oct 21 2016, 21:37:19)
            [GCC 4.8.5 20150623 (Red Hat 4.8.5-4)] on linux
            Type "help", "copyright", "credits" or "license" for more information.
            >>>

    1. list components

            $ sudo yum list rh-python35\*

### fixed

1. [centos 6.5 missing CentOS-Base.repo  file](http://www.linuxquestions.org/questions/linux-newbie-8/deleted-all-of-the-repos-in-yum-repos-d-how-to-restore-them-4175532866/)

    1. nothing to do

            $sudo yum install -y centos-release-SCL
            Loaded plugins: fastestmirror, security
            Loading mirror speeds from cached hostfile
            Setting up Install Process
            No package centos-release-SCL available.
            Error: Nothing to do

            $ ls /etc/yum.repo.d/
            neo4j.repo      os.repo         transwarp.repo

    1. fixed

            $ sudo yum reinstall http://vault.centos.org/6.5/os/x86_64/Packages/centos-release-6-5.el6.centos.11.1.x86_64.rpm

            $ ls /etc/yum.repo.d/
            CentOS-Base.repo         CentOS-SCLo-scl.repo
            CentOS-Debuginfo.repo    CentOS-SCLo-scl-rh.repo
            CentOS-Media.repo        CentOS-Vault.repo

### reference

1. [enable-software-collections-centos](http://xmodulo.com/enable-software-collections-centos.html)

1. [lsb_release](https://unix.stackexchange.com/questions/278061/how-can-i-enable-centos-release-scl-on-centos-6-7)

            $ lsb_release -a
            LSB Version:    :base-4.0-amd64:base-4.0-noarch:core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch
            Distributor ID: CentOS
            Description:    CentOS release 6.5 (Final)
            Release:    6.5
            Codename:   Final
