---
layout: post
title: "monitor remote windows using nagios"
description: ""
category: [windows]
tags: [nagios, windows, nrpe, nsclient++]
---
{% include JB/setup %}


### [monitor remote windows host using nagios](http://tecadmin.net/monitor-remote-windows-host-using-nagios/)

1. setup nsclient++

    1. [download](http://www.nsclient.org/download/) the [latest version](http://files.nsclient.org/released/NSCP-0.4.3.143-x64.msi)

    1. install

    1. config. modules setction check [how to monitor windows servers using nagios and nsclient](http://www.spkaa.com/how-to-monitor-windows-servers-using-nagios-and-nsclient)

            # edit `C:\Porgram Files\NSClient++\nsclient.ini`
            [/settings/default]
            allowed hosts = 192.168.120.151,192.168.120.155

            [/settings/NRPE/server]
            allowed hosts = 192.168.120.151,192.168.120.155
            allow arguments = 1
            allow nasty characters = false
            port = 5666
            use SSL = 1
            insecure = true

            [/modules]
            NRPEListener.dll
            NSClientListener.dll
            FileLogger.dll
            CheckSystem.dll
            CheckDisk.dll
            CheckEventLog.dll
            CheckHelpers.dll
            CheckExternalScripts.dll
            NRPEServer = 1

    1. check use `netstat`

            > netstat -ano | findstr 5666

    1. check use `telnet`

            $ telnet localhost 5666

    1. check use `check_nrpe` if [CHECK_NRPE: Error - Could not complete SSL handshake.](http://serverfault.com/questions/668253/nagios-could-not-complete-ssl-handshake)

            $ /usr/lib/nagios/plugins/check_nrpe -H node1
            I (0.4.3.143 2015-04-29) seem to be doing fine...

            # fixed
            [/settings/NRPE/server]
            insecure = true

1. add host in nagios

    1. add `node1.cfg` to `/usr/local/nagios/etc/servers`

            $ sudo pico node1.cfg
            define hostgroup{
                hostgroup_name        windows-servers
                alias                 windows-servers
            }

            define host{
                use                   windows-server
                host_name             node1
                alias                 node1
                address               192.168.120.151
                max_check_attempts    5
                check_period          24x7
                notification_interval 30
                notification_period   24x7
            }

            define service {
                host_name             node1
                service_description   PING
                check_command         check_ping!100.0,20%!500.0,60%
                check_interval        1
                check_interval        2
                max_check_attempts    4
                retry_interval        2
                notification_interval 2
            }

    1. restart `nagios`

            $ sduo service nagios restart

    1. visit [http://node5/nagios/](http://node5/nagios/)

1. fixed

    1. check uptime

            define service{
                use                   generic-service
                host_name             node1
                service_description   uptime
                check_command         check_nt!UPTIME
            }

            connect to address 192.168.120.151 and port 12489: Connection refused 