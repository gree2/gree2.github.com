---
layout: post
title: "install ubuntu with pxe via osx"
description: ""
category: [setup]
tags: [ubuntu, install, pxe]
---
{% include JB/setup %}


### [info](http://ruby-journal.com/install-ubuntu-with-pxe-via-osx/)

1. `pxe` preboot execution environment

            # is widely used in enterprise environment
            # for mass deployment

            # is your box doesn't have cd/dvd or usb
            # you can install ubuntu using pxe

1. dhcp service

            # pxe-bootable device will look for dhcp service
            # in order to receive available pxe boot server

1. prerequisite

    1. netboot installer for ubuntu

    1. tftp server

    1. a pc that is to be installed to [supports pxe](http://www.tomshardware.com/forum/329569-30-ipv4-ipv6-support)

            # ASUS Z97-K BIOS setup
            => power up
            => press `DEL` or `F2` enter  `UEFI BIOS Utility`
            => press `F7`          enter  `Advanced Mode`
            => press `right arrow` select `Advanced`
            => press `down arrow`  select `Network stack Configuration`
            => press `enter`       enter  `Network stack`
            => press `enter`       choose `Enabled`
            => Ipv4 PXE support    choose `Enabled`
            => Ipv6 PXE support    choose `Disabled`
            => press `F7`          exit   `ExMode`
            => press `F10`                `Save & Exit`

### download ubuntu netboot installer

1. [netboot](http://cdimage.ubuntu.com/netboot/vivid/) choose [amd64 installer](http://archive.ubuntu.com/ubuntu/dists/vivid/main/installer-amd64/current/images/netboot/)

            $ cd ~/Download
            $ curl -O http://archive.ubuntu.com/ubuntu/dists/vivid/main/installer-amd64/current/images/netboot/netboot.tar.gz

1. extract `netboot.tar.gz`

            $ mkdir tftp
            $ tar -zxvf netboot.tar.gz -C tftp

1. fix folder permissions

            $ sudo chown -R hqlgree2@staff .

### setup tftp server

1. easy setup with tftp server software

    1. download [tftp server](http://ww2.unime.it/flr/tftpserver/TftpServer.dmg)

    1. install dmg file

    1. run tftpserver

    1. change root for tftp server

            # /private/tftpboot
            # => /Users/hqlgree2/Downloads/tftp

    1. fixing folder permission

            # working path permission
            # parentals folders permissions

            # you should see `attributes ok`

1. hard setup using built-in tftp server

    1. configure `tftp.plist`

            $ pico /System/Library/LaunchDaemons/tftp.plist

            # change
            <key>Disabled</key>
            <true/>
            # to
            <key>Disabled</key>
            <false/>

            # change
            <string>/private/tftpboot</string>
            # to
            <string>/Users/hqlgree2/Downloads/tftp</string>

    1. start the server

            $ sudo launchctl load -F /System/Library/LaunchDaemons/tftp.plist

            # or

            $ sudo launchctl start com.apple.tftpd

    1. once installation is finished disable it with

            $ sudo launchctl unload -F /System/Library/LaunchDaemons/tftp.plist

            # or

            $ sudo launchctl stop com.apple.tftpd

1. test tftp server

            $ tftp localhost
            tftp> get pxelinux.0
            Received 43574 bytes in 0.0 seconds
            tftp> quit


### setup dhcp server

1. with `isc-dhcp`

    1. install

            $ brew install isc-dhcp
            ==> Downloading http://ftp.isc.org/isc/dhcp/4.3.0/dhcp-4.3.0.tar.gz
            ######################################################################## 100.0%
            ==> ./configure --prefix=/usr/local/Cellar/isc-dhcp/4.3.0 --localstatedir=/usr/local/var/dhcpd
            ==> make -C bind
            ==> make
            ==> make install
            ==> Caveats
            This install of dhcpd expects config files to be in /usr/local/etc.
            All state files (leases and pids) are stored in /usr/local/var/dhcpd.

            Dhcpd needs to run as root since it listens on privileged ports.

            There are two plists because a single dhcpd process may do either
            DHCPv4 or DHCPv6 but not both. Use one or both as needed.

            Note that you must create the appropriate config files before starting
            the services or dhcpd will refuse to run.
              DHCPv4: /usr/local/etc/dhcpd.conf
              DHCPv6: /usr/local/etc/dhcpd6.conf

            Sample config files may be found in /usr/local/etc.

            To have launchd start isc-dhcp at startup:
                sudo cp -fv /usr/local/opt/isc-dhcp/*.plist /Library/LaunchDaemons
                sudo chown root /Library/LaunchDaemons/homebrew.mxcl.isc-dhcp.plist
            Then to load isc-dhcp now:
                sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.isc-dhcp.plist
            ==> Summary
            🍺  /usr/local/Cellar/isc-dhcp/4.3.0: 38 files, 7.2M, built in 7.7 minutes

    1. create configuration file

            $ pico /usr/local/etc/dhcpd.conf
            default-lease-time 600;
            max-lease-time 7200;

            subnet X.X.X.0 netmask Y.Y.Y.0 {
              range X.X.X.151 X.X.X.205;
            }

            option domain-name-servers 8.8.8.8;

            host netbook {
              hardware ethernet ??:??:??:??:??:??;
              filename "pxelinux.0";
              next-server Z.Z.Z.Z; # the IP address of your TFTP server
              fixed-address X.X.X.202;
              option subnet-mask Y.Y.Y.0;
              option broadcast-address X.X.X.255;
              option routers X.X.X.1;
            }

            # X.X.X is network address
            # Y.Y.Y is subnet mask
            # ??:??:??:??:??:?? is mac address of the box
            # Z.Z.Z.Z is address of tftp server

    1. start dhcp server

            $ sudo /usr/local/sbin/dhcpd -f -d -cf /usr/local/etc/dhcpd.conf

    1. once installation finished clean up with

            brew uninstall isc-dhcp

1. with built-in bootpd

    1. configure `/etc/bootpd.plist`

            $ pico /etc/bootpd.plist
            <?xml version="1.0" encoding="UTF-8"?>
            <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
            <plist version="1.0">
            <dict>
                <key>bootp_enabled</key>
                <false/>
                <key>detect_other_dhcp_server</key>
                <integer>1</integer>
                <key>dhcp_enabled</key>
                <array>
                    <string>en0</string>
                </array>
                <key>reply_threshold_seconds</key>
                <integer>0</integer>
                <key>Subnets</key>
                <array>
                    <dict>
                        <key>allocate</key>
                        <true/>
                        <key>lease_max</key>
                        <integer>86400</integer>
                        <key>lease_min</key>
                        <integer>86400</integer>
                        <key>name</key>
                        <string>192.168.1</string>
                        <key>net_address</key>
                        <string>192.168.1.0</string>
                        <key>net_mask</key>
                        <string>255.255.255.0</string>
                        <key>net_range</key>
                        <array>
                            <string>192.168.1.101</string>
                            <string>192.168.1.202</string>
                        </array>
                    </dict>
                </array>
            </dict>
            </plist>

            # network address is 192.168.1.0
            # dhcp allocation pool is 101 to 202

    1. assign static ip address to our to-be-installed host

            $ pico /etc/bootptab
            %%
            # machine entries have the following format:
            #
            # hostname      hwtype  hwaddr              ipaddr          bootfile
            client1         1       00:1f:16:08:61:96   192.168.1.105   pxelinux.0

    1. start the server
    
            $ sudo /bin/launchctl load -w /System/Library/LaunchDaemons/bootps.plist

    1. stop the server once done

            $ sudo /bin/launchctl unload -w /System/Library/LaunchDaemons/bootps.plist

1. [macworld demo](http://hints.macworld.com/article.php?story=20100501044411838)

1. [jacquesf demo](http://www.jacquesf.com/2011/04/mac-os-x-dhcp-server/)

### installation

1. [ubuntu community server and network installation](https://help.ubuntu.com/community/Installation#Server_and_network_installations)

1. [from a local server](https://help.ubuntu.com/community/Installation/LocalNet)

    1. obtain one of the desktop ios

            $ wget http://old-releases.ubuntu.com/releases/9.10/ubuntu-9.10-desktop-amd64.iso

    1. loopback mount the ios

            $ mkdir /mnt/ubuntu
            $ mount -o loop ubuntu-9.10-desktop-amd64.iso /mnt/ubuntu

    1. expose installation media via http server

            # if running apache
            # use a symbolic link
            $ cd /var/www/html
            $ sudo ln -s /mnt/ubuntu ubuntu

    1. test installation directory is accessible by pointing a brower

            $ open http://192.168.100.100/ubuntu

    1. network boot the target machine proceed to the mirrors dialog

            # when asked to select a mirror
            # go to the top of the list and select `enter manually`

            # enter ip address of the http server
            # at the next screen it'll ask for a path
            # enter `/ubuntu`

            # from here on
            # the installation is the same as using the internet mirrors