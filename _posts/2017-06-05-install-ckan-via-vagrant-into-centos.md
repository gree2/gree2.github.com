---
layout: post
title: "install ckan via vagrant into centos"
description: ""
category: [bigdata]
tags: [ckan, centos, vagrant]
---
{% include JB/setup %}


### [steps](https://github.com/ckan/ckan/wiki/How-to-Install-CKAN-2.6.2-on-Vagrant)

1. setup

    1. on host clone ckan code

            $ mkdir ckan-vagrant
            $ cd ckan-vagrant
            $ mkdir etc src
            $ cd src
            $ git clone https://github.com/ckan/ckan.git

    1. on host create Vagrantfile

            $ vagrant init centos/7
            $ nano Vagrantfile
              config.vm.network "forwarded_port", guest: 80, host: 6080
              config.vm.network "forwarded_port", guest: 5000, host: 6050
              config.vm.network "private_network", ip: "192.168.33.60"
              config.vm.synced_folder "etc/", "/etc/ckan/default"
              config.vm.synced_folder "src/", "/usr/lib/ckan/default/src"
              config.vm.provider "virtualbox" do |vb|
                vb.memory = "4096"
              end

    1. ssh into centos 7

            $ vagrant ssh
            $ easy_install pip
            -bash: easy_install: command not found
            $ sudo yum -y update
            $ sudo yum -y install python-pip
            Loaded plugins: fastestmirror
            Loading mirror speeds from cached hostfile
             * base: repos-tx.psychz.net
             * extras: centos.sonn.com
             * updates: mirror.pac-12.org
            No package python-pip available.
            Error: Nothing to do

            $ sudo curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
            $ sudo python get-pip.py

    1. install ckan from source

            $ sudo pip install -e /usr/lib/ckan/default/src/ckan

            $ cd /usr/lib/ckan
            $ pip install virtualenv

    1. setup ~/.bashrc

            # convenience to activate the virtualenv by default
            $ echo . /usr/lib/ckan/default/bin/activate >> ~/.bashrc
            # convenience to default --config value for paster commands
            $ echo export CKAN_INI=/etc/ckan/default/development.ini >> ~/.bashrc
            # convenience to stop nosetests outputting reams of unnecessary logging
            $ echo export NOSE_NOLOGCAPTURE=1 >> ~/.bashrc

    1. generate development.ini

            $ paster make-config ckan /etc/ckan/default/development.ini
            Distribution already installed:
              ckan 2.7.0a0 from /usr/lib/ckan/default/src/ckan
            Creating /etc/ckan/default/development.ini
            Now you should edit the config files
              /etc/ckan/default/development.ini

    1. edit development.ini

            $ nano /etc/ckan/default/development.ini
            ...
            ckan.site_url = http://192.168.33.60:5000

    1. try to start

            $ paster serve /etc/ckan/default/development.ini
            ...
            Problems were found while connecting to the SOLR server
            ...
            could not connect to server: Connection refused
                Is the server running on host "localhost" (127.0.0.1) and accepting
                TCP/IP connections on port 5432

### fixed

1. init error

    1. info

            $ vagrant init centos/7
            Vagrant failed to initialize at a very early stage:

            The plugins failed to initialize correctly. This may be due to manual
            modifications made within the Vagrant home directory. Vagrant can
            attempt to automatically correct this issue by running:

              vagrant plugin repair

            If Vagrant was recently updated, this error may be due to incompatible
            versions of dependencies. To fix this problem please remove and re-install
            all plugins. Vagrant can attempt to do this automatically by running:

              vagrant plugin expunge --reinstall

            Error message given during initialization: Unable to resolve dependency: user requested 'vagrant-cachier (> 0)'

    1. not fixed by

            $ vagrant plugin repair
            Repairing currently installed plugins. This may take a few minutes...
            Failed to automatically repair installed Vagrant plugins. To fix this
            problem remove all user installed plugins and reinstall. Vagrant can
            do this for you automatically by running the following command:

              vagrant plugin expunge --reinstall

            Failure message received during repair:

            Vagrant failed to load a configured plugin source. This can be caused
            by a variety of issues including: transient connectivity issues, proxy
            filtering rejecting access to a configured plugin source, or a configured
            plugin source not responding correctly. Please review the error message
            below to help resolve the issue:

              bad response Not Found 404 (http://ruby.taobao.org/specs.4.8.gz)

            Source: http://ruby.taobao.org/

    1. not fixed by

            $ vagrant plugin expunge --reinstall

            This command permanently deletes all currently installed user plugins. It
            should only be used when a repair command is unable to properly fix the
            system.

            Continue? [N]: y

            All user installed plugins have been removed from this Vagrant environment!

            Vagrant will now attempt to reinstall user plugins that were removed.
            Installing the 'vagrant-cachier' plugin. This can take a few minutes...
            Vagrant failed to load a configured plugin source. This can be caused
            by a variety of issues including: transient connectivity issues, proxy
            filtering rejecting access to a configured plugin source, or a configured
            plugin source not responding correctly. Please review the error message
            below to help resolve the issue:

              bad response Not Found 404 (http://ruby.taobao.org/specs.4.8.gz)

            Source: http://ruby.taobao.org/

    1. fixed by

            $ brew cask uninstall vagrant
            $ brew cask install vagrant

1. This is usually because the filesystem "vboxsf" is not available

    1. info

            $ vagrant up
            ...
            Vagrant was unable to mount VirtualBox shared folders. This is usually
            because the filesystem "vboxsf" is not available. This filesystem is
            made available via the VirtualBox Guest Additions and kernel module.
            Please verify that these guest additions are properly installed in the
            guest. This is not a bug in Vagrant and is usually caused by a faulty
            Vagrant box. For context, the command attempted was:

            mount -t vboxsf -o uid=1000,gid=1000 etc_ckan_default /etc/ckan/default

            The error output from the command was:

            mount: unknown filesystem type 'vboxsf'

    1. fixed by [install vagrant-vbguest](https://blog.mdnsolutions.com/vagrant-up-in-mac-os-vboxsf-file-system-is-not-available/)

            $ vagrant plugin install vagrant-vbguest
            Installing the 'vagrant-vbguest' plugin. This can take a few minutes...
            Fetching: micromachine-2.0.0.gem (100%)
            Fetching: vagrant-vbguest-0.14.2.gem (100%)
            Installed the plugin 'vagrant-vbguest (0.14.2)'!
            hqlgree2:ckan-vagrant hqlgree2$ vagrant up
            Bringing machine 'default' up with 'virtualbox' provider...
            ==> default: Checking if box 'centos/7' is up to date...

1. pg_config executable not found

    1. info

            $ sudo pip install --ignore-installed -r pip-requirements-docs.txt
            Collecting psycopg2==2.4.5 (from -r requirements.txt (line 33))
              Using cached psycopg2-2.4.5.tar.gz
                Complete output from command python setup.py egg_info:
                running egg_info
                creating pip-egg-info/psycopg2.egg-info
                writing pip-egg-info/psycopg2.egg-info/PKG-INFO
                writing top-level names to pip-egg-info/psycopg2.egg-info/top_level.txt
                writing dependency_links to pip-egg-info/psycopg2.egg-info/dependency_links.txt
                writing manifest file 'pip-egg-info/psycopg2.egg-info/SOURCES.txt'
                Error: pg_config executable not found.

                Please add the directory containing pg_config to the PATH
                or specify the full executable path with the option:

                    python setup.py build_ext --pg-config /path/to/pg_config build ...

                or with the pg_config option in 'setup.cfg'.

                ----------------------------------------
            Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-aTTXUH/psycopg2/

    1. fixed by [install python-devel postgresql-devel](https://stackoverflow.com/a/13158616)

            $ sudo yum install python-devel postgresql-devel