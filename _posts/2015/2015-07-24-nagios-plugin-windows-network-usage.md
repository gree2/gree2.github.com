---
layout: post
title: "nagios plugin windows network usage"
description: ""
category: [ubuntu]
tags: [nagios, plugin, network, windows, traffic]
---
{% include JB/setup %}


### [check_win_net_usage](http://www.claudiokuenzler.com/nagios-plugins/check_win_net_usage.php#.VbC3ipP048Y)

1. download from [here](http://www.claudiokuenzler.com/nagios-plugins/check_win_net_usage.sh)

1. requirements

    1. check_nt plugin

    1. nsclient++ installed on windows host

    1. set variable `pluginlocation` to nagios plugin path `/usr/lib/nagios/plugins`

1. usage

            $ ./check_win_net_usage.sh -H node1 -p 5666 -i "Realtek PCIe GBE Family Controller" -o KB

1. definition of the parameters

    1. `-H` hostname of windows server to check

    1. `-p` listening port of nsclient++ on windows server

    1. `-s` password in case nsclient++ was defined to use a password

    1. `-i` name of network interface to use (`not ethX` check windows performance gui)

    1. `-o` choose output of value in KB, MB (default Byte)

1. command definition in `commands.cfg`

    1. output in Bytes/s

            $ sudo pico /usr/local/nagios/etc/objects/commands.cfg
            ...
            define command {
                command_name check_win_net_usage
                command_line $USER1$/check_win_net_usage.sh -H $HOSTADDRESS$ -i $ARG1$ $ARG2$
            }

    1. output in Bytes/s

            $ sudo pico /usr/local/nagios/etc/objects/commands.cfg
            ...
            define command {
                command_name check_win_net_usage
                command_line $USER1$/check_win_net_usage.sh -H $HOSTADDRESS$ -i $ARG1$ -o KB $ARG2$
            }

1. service checks

    1. edit `node1.cfg`

            $ sudo pico /usr/local/nagios/servers/node1.cfg
            ...
            # check windows network
            define service {
                use generic-service
                hostname node1
                service_description network usage
                check_command check_win_net_usage!"Realtek PCIe GBE Family Controller"
            }

    1. edit `node1.cfg` use `port`

            $ sudo pico /usr/local/nagios/servers/node1.cfg
            ...
            # check windows network
            define service {
                use generic-service
                hostname node1
                service_description network usage
                check_command check_win_net_usage!"Realtek PCIe GBE Family Controller"!-p 5666
            }

    1. edit `node1.cfg` use `port, password, format`

            $ sudo pico /usr/local/nagios/servers/node1.cfg
            ...
            # check windows network
            define service {
                use generic-service
                hostname node1
                service_description network usage
                check_command check_win_net_usage!"Realtek PCIe GBE Family Controller"!-p 5666 -s passwd -o MB
            }

1. nagiosgraph `map` entry

            $ sudo pico /usr/local/nagiosgraph/etc/map
            ...
            # service type: check_win_net_usage
            # Regex by Claudio Kuenzler
            # Nagios Output: Network OK - 9 KBytes received/sec, 0 KBytes sent/sec
            # Perfdata: bytes_in=9988;bytes_out=367
            /perfdata:bytes_in=(\d+);bytes_out=(\d+)/
            and push @s, [nt_net,
            ['in_Bps', GAUGE, $1 ],
            ['out_Bps', GAUGE, $2 ] ];

1. how to find name of the network interface

    1. how do you list all instance of a counter

            $ ./check_nrpe -c listCounterInstance -a "Network Interface"

    1. check manually in windows using `performance` application gui

    1. use `typeperf.exe`

            > typeperf.exe -qx | find "Network Interface"

### references

1. [how to monitor windows network traffic with nagios](http://www.claudiokuenzler.com/blog/207/windows-network-traffic-monitoring-with-nagios#.VbCVyJP048Y)

1. [how to install and use nagiosgraph](http://nagios.fm4dd.com/nagiosgraph/nagiosgraph-man.htm)