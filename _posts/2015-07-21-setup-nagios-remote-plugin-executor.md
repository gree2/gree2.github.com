---
layout: post
title: "setup nagios remote plugin executor"
description: ""
category: [ubuntu]
tags: [nagios, nrpe, plugin]
---
{% include JB/setup %}


### config remote hosts

1. install nrpe service

    1. on debian

            $ sudo apt-get install nagios-nrpe-server

    1. centos fedora rhel

            $ yum install nagios-nrpe

1. conf nrpe

    1. config file location

            $ sudo pico /etc/nagios/nrpe.cfg
            ...
            server_port=5666
            ...

1. initiating nrpe service

    1. for redhat based system the nrpe service needs to be added as a startup service

    1. on debian ubuntu or linux mint

            $ sudo service nagios-nrpe-server restart

    1. on centos fedora or rhel

            $ sudo service nrpe restart
            $ chkconfig nrpe on

1. verifying nrpe service status

            $ tail /var/log/syslog

            $ netstat -tpln | grep 5666

### config nagios core server

1. install nrpe plugin

    1. install package

            $ sudo apt-get install nagios-nrpe-plugin

            # redhat based system centos fedora rhel
            $ yum install nagios-plugins-nrpe

    1. edit `check_nrpe` command

            $ sudo pico /etc/nagios-plugins/config/check_nrpe.cfg
            define command{
                command_name check_nrpe
                command_line /usr/lib/nagios/plugins/check_nrpe -H '$HOSTADDRESS$' -c '$ARG1$'
            }

1. defining nagios command for nrpe plugin

            $ sudo pico /usr/local/nagios/etc/object/commands.cfg
            define command{
                command_name check_nrpe
                command_line /usr/lib/nagios/plugins/check_nrpe -H '$HOSTADDRESS$' -c '$ARG1$'
            }

1. adding host and command definition

    1. define remote hosts and commands to execut remotely on them

            # debian ubuntu or linux mint
            $ sudo pico /usr/local/nagios/etc/servers/node2.cfg
            ...
            define service {
                use                    generic-service
                host_name              node2
                service_description    Current Load
                check_command          check_nrpe!check_load
                check_interval         1
            }
            ...

1. restarting nagios service

    1. verify config file

            # on debian ubuntu or linux mint
            $ /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

            # on centos fedora or rhel
            $ nagios -v /etc/nagios/nagios.cfg

    1. restart

            $ sudo service nagios restart

### config custom commands with nrpe

1. setup on remote servers

    1. define commands

            $ sudo pico /etc/nagios/nrpe.cfg
            ...
            ## Warn if number of instances for process_abc exceeds 10. Critical for 20 ##
            command[check_process_abc]=/usr/lib/nagios/plugins/check_procs -w 1:10 -c 1:20 -C process_abc

            ## Critical if the number of instances for process_xyz drops below 1 ##
            command[check_process_xyz]=/usr/lib/nagios/plugins/check_procs -w 1: -c 1: -C process_xyz
            ...

1. setup on nagios core server

    1. to apply the custom commands defined above

            # modify the service definition at nagios core server
            # /etc/local/nagios/etc/objects/nrpe.cfg
            # /etc/nagios/nrpe.cfg
            define service {
                use                    generic-service
                host_name              node2
                service_description    check process abc
                check_command          check_nrpe!check_process_abc
                check_interval         1
            }

            define service {
                use                    generic-service
                host_name              node2
                service_description    check process xyz
                check_command          check_nrpe!check_process_xyz
                check_interval         1
            }

### fixed

1. [CHECK_NRPE: Error - could not complete SSL handshake](http://geekpeek.net/could-not-complete-ssl-handshake/)

    1. this error is usually a client side problem

    1. nagios client holds `nrpe.cfg` usually at `/etc/nagios/nrpe.cfg`

    1. to fix this problem you need to edit this file

            $ sudo pico /etc/nagios/nrpe.cfg
            ...
            allowed_host=127.0.0.1,192.168.120.155

    1. restart nrpe service

            $ sudo /etc/init.d/nagios-nrpe-server restart

    1. test this from nagios server

            $ sudo /usr/lib/nagios/plugins/check_nrpe -H 192.168.120.152
            NRPE v2.15

### references

1. [network bandwidth monitoring nagios way](http://techdire.com/network-bandwidth-monitoring-nagios-way/)

1. [how to monitor remote linux host using nagios 30](http://www.thegeekstuff.com/2008/06/how-to-monitor-remote-linux-host-using-nagios-30/)