---
layout: post
title: "install and configure linux ntp server and client"
description: ""
category: [linux]
tags: [ntp, ntpd, ntpdc, ntpstat]
---
{% include JB/setup %}


### [linux ntp server client](http://www.thegeekstuff.com/2014/06/linux-ntp-server-client/)

1. config ntp server

    1. install ntp server

            $ yum install ntp

    1. setup restrict values

            $ nano /etc/ntp.conf
            restrict default kod nomodify notrap nopeer noquery
            restrict -6 default kod nomodify notrap nopeer noquery

            # `noquery`  prevents dumping status data from `ntpd`
            # `notrap`   prevents control message trap service
            # `nomodify` prevents all ntpq queries that attempts to modify the server
            # `nopeer`   prevents all package that attempts to establish a peer association
            # `kod`      kiss-o-death packet is to be sent to reduce unwanted queries

    1. allow only specific clients

            $ nano /etc/ntp.conf
            restrict 192.168.120.155 mask 255.255.255.0 nomodify notrap
            restrict 192.168.120.154 mask 255.255.255.0 nomodify notrap
            restrict 192.168.120.153 mask 255.255.255.0 nomodify notrap
            restrict 192.168.120.152 mask 255.255.255.0 nomodify notrap
            restrict 192.168.120.151 mask 255.255.255.0 nomodify notrap

    1. add local clock as backup

            $ nano /etc/ntp.conf
            # local clock
            server 127.127.1.0
            fudge 127.127.1.0 stratum 10

    1. setup ntp log

            $ nano /etc/ntp.conf
            logfile /var/log/ntp.log

    1. start ntp server

            $ service ntpd start

1. config ntp client

    1. add entry for your own ntp server

            $ nano /etc/ntp.conf
            server node5 prefer

    1. start ntp daemon

            $ /etc/init.d/ntpd start

    1. [check ntp status](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-Checking_the_Status_of_NTP.html)

            $ ntpq -p
                 remote           refid      st t when poll reach   delay   offset  jitter
            ==============================================================================
            *node5           .LOCL.           1 u   12   16  377    0.072    0.098   0.009


    1. set local date and time

            $ ntpdate -u node5
            # after this initial sync
            # ntp client will talk to the ntp server
            # on an on-going basis
            # to make sure the local time refects the accurate time

    1. get the current status of ntpd

            $ ntpdc -c sysinfo

    1. [to obtain a brief status report from ntpd](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-Checking_the_Status_of_NTP.html)

            $ ntpstat
            synchronised to NTP server (192.168.120.155) at stratum 2 
               time correct to within 13 ms
               polling server every 16 s
