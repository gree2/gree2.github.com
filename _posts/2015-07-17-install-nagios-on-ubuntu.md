---
layout: post
title: "install nagios on ubuntu"
description: ""
category: [ubuntu]
tags: [nagios, plugin, ubuntu, nrpe, yum, centos]
---
{% include JB/setup %}

### [info](https://www.nagios.org/)

1. with nagios you can

    1. monitor your entire it infrastructure

    1. spot problem before they occur

    1. know immediately when problem arise

    1. share avaiability data with stakeholders

    1. detect securiry breaches

    1. plan and budget for it upgrades

    1. reduce downtime and business losses

### [install](http://www.unixmen.com/install-configure-nagios-4-ubuntu-14-1014-04/)

1. prerequisites

    1. fully working lamp stack

    1. install the following prerequisites

            $ sudo apt-get install build-essential libgd2-xpm-dev apache2-utils

1. create nagios user and group

    1. create `nagios` user with a password

            $ sudo useradd -m nagios
            $ sudo passwd nagios

    1. create `nagcmd` group for allowing external commands to be submitted through webui

            $ sudo groupadd nagcmd
            # add both `nagios` and `apache` user to the group
            $ sudo usermod -a -G nagcmd nagios
            $ sudo usermod -a -G nagcmd www-data

1. download nagios and plugins

            $ wget http://prdownloads.sourceforge.net/sourceforge/nagios/nagios-4.0.8.tar.gz
            # --2015-07-17 08:57:11--  http://prdownloads.sourceforge.net/sourceforge/nagios/nagios-4.0.8.tar.gz
            # Resolving prdownloads.sourceforge.net... 54.192.69.238, 54.230.70.184, 54.230.70.69, ...
            # Connecting to prdownloads.sourceforge.net|54.192.69.238|:80... connected.
            # HTTP request sent, awaiting response... 403 Forbidden
            # 2015-07-17 08:57:12 ERROR 403: Forbidden.

            $ wget https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.0.8.tar.gz

            $ wget http://nagios-plugins.org/download/nagios-plugins-2.0.3.tar.gz

1. install nagios and plugin

    1. install nagios

            $ tar -zxf nagios-4.0.8.tar.gz
            $ cd nagios-4.0.8

            # compile and install nagios
            $ sudo ./configure --with-command-group=nagcmd
            $ sudo make all
            ###################
            $ sudo make install
            ...
            You can continue with installing Nagios as follows (type 'make'
            without any arguments for a list of all possible options):

              make install-init
                 - This installs the init script in /etc/init.d

              make install-commandmode
                 - This installs and configures permissions on the
                   directory for holding the external command file

              make install-config
                 - This installs sample config files in /usr/local/nagios/etc

            make[1]: Leaving directory '/home/node5/Downloads/nagioscore-nagios-4.0.8'
            
            ########################
            $ sudo make install-init
            /usr/bin/install -c -m 755 -d -o root -g root /etc/init.d
            /usr/bin/install -c -m 755 -o root -g root daemon-init /etc/init.d/nagios
            
            *** Init script installed ***

            ##########################
            $ sudo make install-config
            /usr/bin/install -c -m 775 -o nagios -g nagios -d /usr/local/nagios/etc
            /usr/bin/install -c -m 775 -o nagios -g nagios -d /usr/local/nagios/etc/objects
            /usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/nagios.cfg /usr/local/nagios/etc/nagios.cfg
            /usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/cgi.cfg /usr/local/nagios/etc/cgi.cfg
            /usr/bin/install -c -b -m 660 -o nagios -g nagios sample-config/resource.cfg /usr/local/nagios/etc/resource.cfg
            /usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/templates.cfg /usr/local/nagios/etc/objects/templates.cfg
            /usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/commands.cfg /usr/local/nagios/etc/objects/commands.cfg
            /usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/contacts.cfg /usr/local/nagios/etc/objects/contacts.cfg
            /usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/timeperiods.cfg /usr/local/nagios/etc/objects/timeperiods.cfg
            /usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/localhost.cfg /usr/local/nagios/etc/objects/localhost.cfg
            /usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/windows.cfg /usr/local/nagios/etc/objects/windows.cfg
            /usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/printer.cfg /usr/local/nagios/etc/objects/printer.cfg
            /usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/switch.cfg /usr/local/nagios/etc/objects/switch.cfg
            
            *** Config files installed ***
            
            Remember, these are *SAMPLE* config files.  You'll need to read
            the documentation for more information on how to actually define
            services, hosts, etc. to fit your particular needs.

            ###############################
            $ sudo make install-commandmode
            /usr/bin/install -c -m 775 -o nagios -g nagcmd -d /usr/local/nagios/var/rw
            chmod g+s /usr/local/nagios/var/rw
            
            *** External command directory configured ***

    1. install nagios web interface

            # compile and install nagios web interface
            $ sudo make install-webconf
            # you may get the following error
            /usr/bin/install -c -m 644 sample-config/httpd.conf /etc/httpd/conf.d/nagios.conf
            /usr/bin/install: cannot create regular file ‘/etc/httpd/conf.d/nagios.conf’: No such file or directory
            Makefile:296: recipe for target 'install-webconf' failed
            make: *** [install-webconf] Error 1

            # so run the following command instead of using previous one
            $ sudo /usr/bin/install -c -m 644 sample-config/httpd.conf /etc/httpd/conf.d/nagios.config

            # check
            $ sudo ls -l /etc/apache2/sites-enabled/

            # create a `nagiosadmin` account for logging into the nagios web interface
            $ sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
            # restart apache to make setting take effect
            $ sudo service apache2 restart

    1. install nagios plugins

            $ tar -zxf nagios-plugins-2.0.3.tar.gz
            $ cd nagios-plugins-2.0.3

            # compile and install
            $ sudo ./configure --with-nagios-user=nagios --with-nagios-group=nagios
            # ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            #                      check additional tip
            $ sudo make
            $ sudo make install

1. configure nagios

    1. config email id to receive alerts

            $ sudo pico /usr/local/nagios/etc/object/contacts.cfg
            ...
            define contact{
                contact_name                    nagiosadmin             ; Short name of user
                use                             generic-contact         ; Inherit default values from generic-contact template (defined above)
                alias                           Nagios Admin            ; Full name of user

                email                           aa@bb.com  ; <<***** CHANGE THIS TO YOUR EMAIL ADDRESS ******
            }
            ...

    1. nagios administrative access

            $ sudo pico /etc/apache2/sites-enabled/nagios.conf
            ...
            Deny from all
            Allow from 127.0.0.1 192.168.1.0/24
            ...

    1. enable apache's rewrite and cgi modules

            $ sudo a2enmod rewrite
            $ sudo a2enmod cgi

    1. check `nagios.conf` syntax errors

            $ sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

    1. start nagios service and make it autostart

            $ sudo service nagios start
            Failed to start nagios.service: Unit nagios.service failed to load: No such file or directory.
            # ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            #                      check additional tip

            $ sudo ln -s /etc/init.d/nagios /etc/rcS.d/S99nagios

1. access nagios web interface

            http://nagios-server-ip/nagios

### monitor

1. install `nrpe` and `nagios-plugins` in our monitoring targets

            # CentOS/RHEL/Scientific Linux clients
            # add epel repo in your clients to install nrpe package

            # on CentOS 7
            $ yum install epel-release

            # on CentOS 6.x
            # refer http://www.unixmen.com/install-epel-repository-rhel-centos-scientific-linux-6/

            # install `nrpe` and `nagios-plugins`
            $ yum install nrpe nagios-plugins-all openssl

            # on debian/ubuntu clients
            $ sudo apt-get install nagios-nrpe-server nagios-plugins

1. [install](http://askubuntu.com/questions/490740/nagios-nrpe-dameon-check-nrpe-plugin-missing) `nagios-nrpe-plugin`

            $ sudo apt-get install nagios-nrpe-plugin
            $ cd /usr/lib/nagios/plugins
            $ ls check_nr*

1. config monitoring targets

    1. edit `/etc/nagios/nrpe.cfg`

            $ sudo pico /etc/nagios/nrpe.cfg
            ...
            ## find the following line and add the nagios server ip
            allowed_hosts=127.0.0.1 192.168.120.155

    1. start nrpe service

            # on centos clients
            # centos 7
            $ systemctl start nrpe
            $ chkconfig nrpe on

            # centos 6.x
            $ service nrpe start
            $ chkconfig nrpe on

            # on debian/ubuntu clients
            $ sudo /etc/init.d/nagios-nrpe-server restart

1. `nagios server` config

    1. add the clients in config file

            $ sudo pico /usr/local/nagios/etc/nagios.cfg
            ...
            ## find and uncomment the following line
            cfg_dir=/usr/local/nagios/etc/servers

            # create a directory `servers` under `/usr/local/nagios/ect`
            $ sudo mkdir /usr/local/nagios/etc/servers

            # create config file to the client to be monitored
            $ sudo pico /usr/local/nagios/etc/servers/clients.cfg
            define host{
                use                   linux-server
                host_name             node3
                alias                 node3
                address               192.168.120.153
                max_check_attempts    5
                check_period          24x7
                notification_interval 30
                notification_period   24x7
            }

    1. restart nagios service

            $ sudo service nagios restart
            # wait for few seconds
            # refresh nagios admin console in browser
            # navigate to `Hosts`

### define services

1. monitor `ssh` service

    1. edit `/usr/local/nagios/etc/servers/clients.cfg`

            $ sudo pico /usr/local/nagios/etc/servers/clients.cfg
            define host{
                use                   linux-server
                host_name             node3
                alias                 node3
                address               192.168.120.153
                max_check_attempts    5
                check_period          24x7
                notification_interval 30
                notification_period   24x7
            }

            define service {
                use                   generic-service
                host_name             node3
                service_description   SSH
                check_command         check_ssh
                notifications_enabled 0
            }

    1. restart nagios service

            $ sudo service nagios restart
            # wait for few seconds
            # refresh nagios admin console in browser
            # navigate to `Services`

### additional tip

1. trying to use `check_http` with `-S` flag (for https)

    1. make sure install `openssl` and `libssl-dev` first

            # even if nagios server is checking a remote client
            # you need `openssl` and `libssl-dev` locally

    1. configure nagios plugins add `--with-openssl`

            $ sudo ./configure --with-nagios-user=nagios --with-nagios-group=nagios --with-openssl

1. [Failed to start nagios.service: Unit nagios.service failed to load: No such file or directory.](http://www.ubuntugeek.com/install-nagios-4-0-8-on-ubuntu-15-04-vivid-vervet-server.html)

    1. solution 1 not fixed

            # copy `/etc/init.d/skeleton` to `/etc/init.d/nagios`
            $ cp /etc/init.d/skeleton /etc/init.d/nagios

            # edit it
            $ sudo pico /etc/init.d/nagios
            DESC="Nagios"
            NAME=nagios
            DAEMON=/usr/local/nagios/bin/$NAME
            DAEMON_ARGS="-d /usr/local/nagios/etc/nagios.cfg"
            PIDFILE=/usr/local/nagios/var/$NAME.lock

            # change permissions
            $ sudo chmod +x /etc/init.d/nagios

            # start nagios using
            $ /etc/init.d/nagios start

    1. solution 2 fixed

            $ reboot

1. references

    1. [Nagios 4.0.1 Released – Install on RHEL/CentOS 6.x/5.x and Fedora 19/18/17](http://www.tecmint.com/install-nagios-in-linux/)

    1. [Nagios 4.0.8 installation and configuration in Ubuntu/Debian/Linux Mint](http://www.2daygeek.com/nagios-4-0-8-installation-ubuntu-debian-linuxmint/)

    1. [How To Install Nagios On Ubuntu 12.10](https://www.digitalocean.com/community/tutorials/how-to-install-nagios-on-ubuntu-12-10)

    1. [Install And Configure Nagios 4 On Ubuntu 14.10/14.04](http://www.unixmen.com/install-configure-nagios-4-ubuntu-14-1014-04/)

    1. [How To Install LAMP Stack On Ubuntu 15.04](http://www.unixmen.com/how-to-install-lamp-stack-on-ubuntu-15-04/)

    1. [How to install Nagios 4 from source on Ubuntu 14.04 LTS](http://sharadchhetri.com/2014/08/25/install-nagios-4-source-ubuntu-14-04-lts/)

    1. [nagios installation procedure in fedora centos](http://www.linuxfunda.com/2012/10/21/nagios-installation-procedure-in-fedoracentos/)

    1. [nagios core enabling external commands](http://theurbanpenguin.com/wp/index.php/nagios-core-enabling-external-commands/)