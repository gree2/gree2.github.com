---
layout: post
title: "mac network commands cheat sheet"
description: ""
category: [mac]
tags: [network]
---
{% include JB/setup %}


### ipconfig

1. get `ip address`

            $ ipconfig getifaddr en0
            $ ip=`ipconfig getifaddr en0`; echo $ip
            192.168.1.154

1. get `subnet mask`

            $ ipconfig getoption en0 subnet_mask
            255.255.255.0

1. get `dns server`

            $ ipconfig getoption en0 domain_name_server
            192.168.1.253

1. get `info about how en0 got its dhcp on`

            $ ipconfig getpacket en0
            op = BOOTREPLY
            htype = 1
            flags = 0
            hlen = 6
            hops = 0
            xid = 765413854
            secs = 0
            ciaddr = 192.168.1.154
            yiaddr = 192.168.1.154
            siaddr = 0.0.0.0
            giaddr = 0.0.0.0
            chaddr = 34:36:3b:cd:fd:ae
            sname = 
            file = 
            options:
            Options count is 7
            dhcp_message_type (uint8): ACK 0x5
            server_identifier (ip): 192.168.1.253
            lease_time (uint32): 0x1c20
            subnet_mask (ip): 255.255.255.0
            router (ip_mult): {192.168.1.253}
            domain_name_server (ip_mult): {192.168.1.253}
            end (none): 

1. [renew dhcp lease](http://osxdaily.com/2010/09/01/set-ip-address-from-the-mac-command-line/)

            $ ipconfig set en0 DHCP

1. [set a specific ip address](http://osxdaily.com/2010/09/01/set-ip-address-from-the-mac-command-line/)

            $ ipconfig set en0 INFORM 192.168.1.160

### ifconfig

1. get `network info`

            $ ifconfig en0
            en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
                ether 34:36:3b:cd:fd:ae 
                inet6 fe80::3636:3bff:fecd:fdae%en0 prefixlen 64 scopeid 0x4 
                inet 192.168.1.154 netmask 0xffffff00 broadcast 192.168.1.255
                nd6 options=1<PERFORMNUD>
                media: autoselect
                status: active

1. set `ip address` and `netmask`

            $ ifconfig en0 inet 192.168.1.154 netmask 255.255.255.0

1. renew `dhcp leases`

            $ ipconfig set en0 BOOTP && ipconfig set en0 DHCP
            $ ifconfig en0 down && ifconfig en0 up

### networksetup

1. get a list of `location` on the computer

            $ networksetup -listlocations
            Automatic

1. get `active location`

            $ networksetup -getcurrentlocation
            Automatic

1. config manual `static ip address`

            $ networksetup -setmanual Wi-Fi 192.168.1.154 255.255.255.0 192.168.1.253

1. config `dns server`

            $ networksetup -setdnsservers Wi-Fi 192.168.1.154 192.168.1.253

1. get `dns server`

            $ networksetup -getdnsservers Wi-Fi

### firewall

1. stop the application layer firewall

            $ launchctl unload /System/Library/LaunchAgents/com.apple.alf.useragent.plistlaunchctl unload /System/Library/LaunchAgents/com.apple.alf.agent.plist

1. start the application layer firewall

            $ launchctl load /System/Library/LaunchAgents/com.apple.alf.agent.plistlaunchctl load /System/Library/LaunchAgents/com.apple.alf.useragent.plist

1. allow an app to communicate outside through the application layer firewall

            $ socketfilterfw -t "/Applications/..."

### route

1. routing table

            $ netstat -nr

1. add a route

            # so that traffice for 192.168.1.0/24
            # communicates over the 192.168.1.253
            $ sudo route -n add 192.168.1.0/24 192.168.1.253

1. delete a route

            $ sudo route -n delete 192.168.1.0/24 192.168.1.253

### netstat

1. view info on all sockets

            $ netstat -at

1. network info for ipv6

            $ netstat -lt

1. per protocol network statistics

            $ netstat -s

1. statistics for a specific network protocol

            $ netstat -p igmp

1. statistics for network interfaces

            $ netstat -i

1. view network info as it happens

            $ ntop

### ping

1. put a delay in pings

            $ ping -i 5 192.168.1.253

1. ping hostname 5 times and then stop

            $ ping -c 5 192.168.1.253

1. flood ping the host

            $ ping -f location

1. set packet size during ping

            $ ping -s 100 192.168.1.253

1. customize source ip during ping

            $ ping -S 192.168.1.160 192.168.1.253

### trace

1. trace the path packets go through

            $ traceroute baidu.com

1. without looking up names

            $ traceroute -n baidu.com

1. in debug mode

            $ traceroute -d baidu.com

### nc

1. establish a network connection

            $ nc -v baidu.com 80

1. establish a network connection over port 2195

            $ nc -v -w 15 baidu.com 2196

1. establish a network connection only allowing ipv4

            $ nc -v -4 baidu.com 2196

1. setup a network listener on port 2196 for testing

            $ nc -l 2196

### tcpdump

1. capture some packets

            $ tcpdump -nS

1. capture all packets

            $ tcpdump-nnvvXS

1. capture packets for port

            $ tcpdump -nnvvXs 548

1. capture all packets for a given port going to destination 192.168.1.160

            $ tcpdump -nnvvXs 548 dst 192.168.1.160

1. capture packets as above but dump to a pcap file

            $ tcpdump -nnvvXs 548 dst 192.168.1.160 -w /tmp/demo.pcap

1. read tcpdump (cap) files and make them human readable

            $ tcpdump -qns 0 -A -r /tmp/demo.pcap

### other

1. flush the dns cache

            $ dscacheutil -flushcache

1. clear arp cache

            $ arp -ad

1. what binaries have what ports and in what states are those ports

            $ lsof -n -i4TCP

1. make an alias for looking at what has a listener open, called ports

            $ alias ports='lsof -n -i4TCP | grep LISTEN'

1. edit hosts file

            $ sudo pico /private/etc/hosts

### references

1. [mac network commands cheat sheet](http://krypted.com/mac-security/mac-network-commands-cheat-sheet/)