---
layout: post
title: "oracle vm virtualbox networking"
description: ""
category: [virtualbox]
tags: [networking, nat, host-only]
---
{% include JB/setup %}


### config

1. file -> preferences -> network

    1. nat networks

            # click `adds new nat network`
            make sure newly added nat network's `active` checked

    1. host-only networks

            virtualbox host-only ethernet adapter

1. vm -> settings

    1. network adapter 1

            check `enable network adapter`
            attached to: nat network
            name:        natnetwork

            advanced
            adapter type:     intel pro/1000 mt desktop (82540em)
            promiscuous mode: allow all
            mac address:      08002776cac1
            check `cable connected`

    1. network adapter 2

            check `enable network adapter`
            attached to: host-only adapter
            name:        virtualbox host-only ethernet adapter

            advanced
            adapter type:     intel pro/1000 mt desktop (82540em)
            promiscuous mode: allow all
            mac address:      080027f49269
            check `cable connected`

### config interfaces

            $ sudo pico /etc/network/interfaces
            ...

            auto lo
            iface lo inet loopback

            auto eth0
            iface eth0 inet static
            address 192.168.56.100
            netmask 255.255.255.0
            gateway 192.168.1.253

            auto eth1
            iface eth1 inet dhcp
