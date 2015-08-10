---
layout: post
title: "vagrant config networking"
description: ""
category: [auto]
tags: [vagrant, networking, port, collision, forwarding]
---
{% include JB/setup %}


### port forwarding

1. demo

    1. Vagrantfile

            Vagrant.configure(2) do |config|
                config.vm.box = "ubuntu/trusty64"
                config.vm.network :forwarding_port, guest: 80, host: 8800
            end

    1. boot messages

            ==> default: Forwarding ports...
                default: 80 => 8800 (adapter 1)
                default: 22 => 2222 (adapter 1)

1. forbid remote access to the forwarded port

            host_ip: "127.0.0.1"

            # config.vm.network :forwarding_port, guest: 80, host: 8800
            # this configuration allows everyone who knows your ip address
            # to access your web server

### port collision

1. demo

    1. autocorrecting port collision

            Vagrant.configure(2) do |config|
                config.vm.box = "ubuntu/trusty64"
                config.vm.network :forwarded_port, guest: 80, host: 8800, auto_correct: true
            end

### private networks

1. demo

    1. manual configuration

            Vagrant.configure(2) do |config|
                config.vm.box = "ubuntu/trusty64"
                config.vm.network "private_network", ip: "192.168.100.100"
            end

    1. dhcp configuration

            Vagrant.configure(2) do |config|
                config.vm.box = "ubuntu/trusty64"
                config.vm.network "private_network", type: "dhcp"
            end

1. 2 guest use private network

    1. vbox1

            # 1. `Vagrantfile`
            Vagrant.configure(2) do |config|
                config.vm.box = "ubuntu/trusty64"
                config.vm.network "private_network", ip: "192.168.200.100"
            end

            # 2. host
            $ vagrant up
            Bringing machine 'default' up with 'virtualbox' provider...
            ==> default: Importing base box 'ubuntu/trusty64'...
            ==> default: Matching MAC address for NAT networking...
            ==> default: Setting the name of the VM: vbox1_default_1439187398545_3098
            ==> default: Clearing any previously set forwarded ports...
            ==> default: Clearing any previously set network interfaces...
            ==> default: Preparing network interfaces based on configuration...
                default: Adapter 1: nat
                default: Adapter 2: hostonly
            ...

            # 3. host
            $ vagrant ssh
            # 4. guest
            $ ifconfig
            eth0      Link encap:Ethernet  HWaddr 08:00:27:3a:0b:5b  
                      inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
                      inet6 addr: fe80::a00:27ff:fe3a:b5b/64 Scope:Link
                      UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
                      RX packets:622 errors:0 dropped:0 overruns:0 frame:0
                      TX packets:446 errors:0 dropped:0 overruns:0 carrier:0
                      collisions:0 txqueuelen:1000 
                      RX bytes:71065 (71.0 KB)  TX bytes:60339 (60.3 KB)

            eth1      Link encap:Ethernet  HWaddr 08:00:27:a4:9b:c3  
                      inet addr:192.168.200.100  Bcast:192.168.200.255  Mask:255.255.255.0
                      inet6 addr: fe80::a00:27ff:fea4:9bc3/64 Scope:Link
                      UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
                      RX packets:0 errors:0 dropped:0 overruns:0 frame:0
                      TX packets:8 errors:0 dropped:0 overruns:0 carrier:0
                      collisions:0 txqueuelen:1000 
                      RX bytes:0 (0.0 B)  TX bytes:648 (648.0 B)

            lo        Link encap:Local Loopback  
                      inet addr:127.0.0.1  Mask:255.0.0.0
                      inet6 addr: ::1/128 Scope:Host
                      UP LOOPBACK RUNNING  MTU:65536  Metric:1
                      RX packets:0 errors:0 dropped:0 overruns:0 frame:0
                      TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
                      collisions:0 txqueuelen:0 
                      RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)


    1. vbox2

            # 1. `Vagrantfile`
            Vagrant.configure(2) do |config|
                config.vm.box = "ubuntu/trusty64"
                config.vm.network "private_network", ip: "192.168.100.100"
            end

            # 2. host
            $ vagrant up
            ==> default: Importing base box 'ubuntu/trusty64'...
            ==> default: Matching MAC address for NAT networking...
            ==> default: Setting the name of the VM: vbox2_default_1439187509548_65472
            ==> default: Clearing any previously set forwarded ports...
            ==> default: Fixed port collision for 22 => 2222. Now on port 2200.
            ==> default: Clearing any previously set network interfaces...
            ==> default: Preparing network interfaces based on configuration...
                default: Adapter 1: nat
                default: Adapter 2: hostonly
            ...

            # 3. host
            $ vagrant ssh
            # 4. guest
            $ ifconfig
            eth0      Link encap:Ethernet  HWaddr 08:00:27:3a:0b:5b  
                      inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
                      inet6 addr: fe80::a00:27ff:fe3a:b5b/64 Scope:Link
                      UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
                      RX packets:623 errors:0 dropped:0 overruns:0 frame:0
                      TX packets:444 errors:0 dropped:0 overruns:0 carrier:0
                      collisions:0 txqueuelen:1000 
                      RX bytes:71125 (71.1 KB)  TX bytes:60108 (60.1 KB)

            eth1      Link encap:Ethernet  HWaddr 08:00:27:0a:35:67  
                      inet addr:192.168.100.100  Bcast:192.168.100.255  Mask:255.255.255.0
                      inet6 addr: fe80::a00:27ff:fe0a:3567/64 Scope:Link
                      UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
                      RX packets:0 errors:0 dropped:0 overruns:0 frame:0
                      TX packets:8 errors:0 dropped:0 overruns:0 carrier:0
                      collisions:0 txqueuelen:1000 
                      RX bytes:0 (0.0 B)  TX bytes:648 (648.0 B)

            lo        Link encap:Local Loopback  
                      inet addr:127.0.0.1  Mask:255.0.0.0
                      inet6 addr: ::1/128 Scope:Host
                      UP LOOPBACK RUNNING  MTU:65536  Metric:1
                      RX packets:0 errors:0 dropped:0 overruns:0 frame:0
                      TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
                      collisions:0 txqueuelen:0 
                      RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

    1. ifconfig on host when vbox1 up

            # host
            $ ifconfig
            ...
            vboxnet0: flags=8842<BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
                ether 0a:00:27:00:00:00 
            vboxnet1: flags=8842<BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
                ether 0a:00:27:00:00:01 
            vboxnet2: flags=8943<UP,BROADCAST,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
                ether 0a:00:27:00:00:02 
                inet 192.168.200.1 netmask 0xffffff00 broadcast 192.168.200.255

    1. ifconfig on host when vbox2 up

            # host
            $ ifconfig
            vboxnet0: flags=8842<BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
                ether 0a:00:27:00:00:00 
            vboxnet1: flags=8842<BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
                ether 0a:00:27:00:00:01 
            vboxnet2: flags=8943<UP,BROADCAST,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
                ether 0a:00:27:00:00:02 
                inet 192.168.200.1 netmask 0xffffff00 broadcast 192.168.200.255
            vboxnet3: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
                ether 0a:00:27:00:00:03 
                inet 192.168.100.1 netmask 0xffffff00 broadcast 192.168.100.255

1. remove interfaces from host

    1. destroy guest machine

            # host
            $ cd vbox1
            $ vagrant destroy
            $ cd ../vbox2
            $ vagrant destroy

    1. remove interfaces

            # host
            $ VBoxManage hostonlyif remote vboxnet2
            $ VBoxManage hostonlyif remote vboxnet3

### public network

1. demo

    1. Vagrantfile

            # 1. host
            Vagrant.configure(2) do |config|
                config.vm.box = "ubuntu/trusty64"
                config.vm.network "public_network"
            end

            # 2. host
            $ vagrant up
            Bringing machine 'default' up with 'virtualbox' provider...
            ==> default: Importing base box 'ubuntu/trusty64'...
            ==> default: Matching MAC address for NAT networking...
            ==> default: Setting the name of the VM: vbox3_default_1439189338131_96059
            ==> default: Clearing any previously set forwarded ports...
            ==> default: Clearing any previously set network interfaces...
            ==> default: Available bridged network interfaces:
            1) en0: Wi-Fi (AirPort)
            2) en1: Thunderbolt 1
            3) en2: Thunderbolt 2
            4) p2p0
            5) awdl0
            6) bridge0
            ==> default: When choosing an interface, it is usually the one that is
            ==> default: being used to connect to the internet.
                default: Which interface should the network bridge to? 1
            ==> default: Preparing network interfaces based on configuration...
                default: Adapter 1: nat
                default: Adapter 2: bridged
            ...

            # 3. guest
            $ ifconfig
            eth0      Link encap:Ethernet  HWaddr 08:00:27:3a:0b:5b  
                      inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
                      inet6 addr: fe80::a00:27ff:fe3a:b5b/64 Scope:Link
                      UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
                      RX packets:646 errors:0 dropped:0 overruns:0 frame:0
                      TX packets:478 errors:0 dropped:0 overruns:0 carrier:0
                      collisions:0 txqueuelen:1000 
                      RX bytes:72324 (72.3 KB)  TX bytes:63887 (63.8 KB)

            eth1      Link encap:Ethernet  HWaddr 08:00:27:b3:e1:cb  
                      inet addr:192.168.1.9  Bcast:192.168.1.255  Mask:255.255.255.0
                      inet6 addr: fe80::a00:27ff:feb3:e1cb/64 Scope:Link
                      UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
                      RX packets:7 errors:0 dropped:0 overruns:0 frame:0
                      TX packets:10 errors:0 dropped:0 overruns:0 carrier:0
                      collisions:0 txqueuelen:1000 
                      RX bytes:2044 (2.0 KB)  TX bytes:1332 (1.3 KB)

            lo        Link encap:Local Loopback  
                      inet addr:127.0.0.1  Mask:255.0.0.0
                      inet6 addr: ::1/128 Scope:Host
                      UP LOOPBACK RUNNING  MTU:65536  Metric:1
                      RX packets:0 errors:0 dropped:0 overruns:0 frame:0
                      TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
                      collisions:0 txqueuelen:0 
                      RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

    1. use `bridge` parameter

            # 1. host
            config.vm.network "public_network", bridge: 'en1: Wi-Fi (AirPort)'

            # 2. host
            $ vagrant up
            Bringing machine 'default' up with 'virtualbox' provider...
            ==> default: Importing base box 'ubuntu/trusty64'...
            ==> default: Matching MAC address for NAT networking...
            ==> default: Setting the name of the VM: vbox3_default_1439189742526_49874
            ==> default: Clearing any previously set forwarded ports...
            ==> default: Fixed port collision for 22 => 2222. Now on port 2200.
            ==> default: Clearing any previously set network interfaces...
            ==> default: Preparing network interfaces based on configuration...
                default: Adapter 1: nat
                default: Adapter 2: bridged

    1. not use dhcp and provide an ip

            config.vm.network "public_network", ip: "192.168.1.10"
