---
layout: post
title: "ssh possible break in attempt"
description: ""
category: [centos]
tags: [ssh, sshd_config, usedns]
---
{% include JB/setup %}


### steps

1. ssh server: check sshd status

        $ systemctl status sshd -l
        ● sshd.service - OpenSSH server daemon
           Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
           Active: active (running) since 一 2018-06-11 21:46:11 CST; 2s ago
             Docs: man:sshd(8)
                   man:sshd_config(5)
         Main PID: 10463 (sshd)
           Memory: 2.2M
           CGroup: /system.slice/sshd.service
                   ├─10422 sshd: [accepted]
                   ├─10423 sshd: [net]
                   └─10463 /usr/sbin/sshd -D

        6月 11 21:46:11 dc systemd[1]: Starting OpenSSH server daemon...
        6月 11 21:46:11 dc sshd[10463]: Server listening on 0.0.0.0 port 22.
        6月 11 21:46:11 dc sshd[10463]: Server listening on :: port 22.
        6月 11 21:46:11 dc systemd[1]: Started OpenSSH server daemon.

1. remote box: login

        $ ssh dc
        Last login: Mon Jun 11 21:42:56 2018 from 10.1.1.3

1. ssh server: check status again

        $ systemctl status sshd -l
        ● sshd.service - OpenSSH server daemon
           Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
           Active: active (running) since 一 2018-06-11 21:46:11 CST; 25s ago
             Docs: man:sshd(8)
                   man:sshd_config(5)
         Main PID: 10463 (sshd)
           Memory: 3.5M
           CGroup: /system.slice/sshd.service
                   ├─10422 sshd: [accepted]
                   ├─10423 sshd: [net]
                   └─10463 /usr/sbin/sshd -D

        6月 11 21:46:11 dc systemd[1]: Starting OpenSSH server daemon...
        6月 11 21:46:11 dc sshd[10463]: Server listening on 0.0.0.0 port 22.
        6月 11 21:46:11 dc sshd[10463]: Server listening on :: port 22.
        6月 11 21:46:11 dc systemd[1]: Started OpenSSH server daemon.
        6月 11 21:46:19 dc sshd[10479]: reverse mapping checking getaddrinfo for bogon [10.1.1.3] failed - POSSIBLE BREAK-IN ATTEMPT!
                                                                                                           ^^^^^^^^^^^^^^^^^^^^^^^^^^

1. remote box: login again

        $ ssh dc
        no response here
        ^^^^^^^^^^^^^^^^

### fixed

1. ["reverse mapping checking getaddrinfo" and "POSSIBLE BREAK-IN ATTEMPT" error messages via SSH](https://access.redhat.com/solutions/83933)

    1. 3 ways to resolve this issue

            1. Setup a Reverse DNS Record (aka PTR Record) for the SSH client.
            2. Ensure UseDNS no and GSSAPIAuthentication no are set in /etc/ssh/sshd_config on the SSH server, then restart the sshd.
            3. Confirm that /etc/hosts on the SSH server has an entry for the SSH client IP address and hostname.

    1. edit /etc/sshd/sshd_config

            $ nano /etc/sshd/sshd_config
            ...
            # line 93
            GSSAPIAuthentication no
            ...
            # line 129
            UseDNS no
            ...

    1. restart sshd

            $ systemctl restart sshd
