---
layout: post
title: "how to setup a usb key to install centos"
description: ""
category: [linux]
tags: [usb, centos, dd, df, diskutil, mount, unmount]
---
{% include JB/setup %}


### [how to setup a usb key to install centos](https://wiki.centos.org/HowTos/InstallFromUSBkey)

1. steps

    1. info

            starting with centos 6.5
            you can install from usb keys
            by simply transferring the desired iso using dd

    1. demo

            $ sudo dd if=CentOS-6.5-x86_64-bin-DVD1.iso of=/dev/sdb

### fixed

1. dd: /Volumes/gree2: Is a directory

    1. info

            $ dd if=CentOS-6.5-x86_64-bin-DVD1.iso of=/Volumes/gree2
            dd: /Volumes/gree2: Is a directory

    1. check with `mount`

            $ mount
            /dev/disk1 on / (hfs, local, journaled)
            devfs on /dev (devfs, local, nobrowse)
            map -hosts on /net (autofs, nosuid, automounted, nobrowse)
            map auto_home on /home (autofs, automounted, nobrowse)
            /dev/disk2s4 on /Volumes/gree2 (msdos, local, nodev, nosuid, noowners)

    1. fixed

            use /dev/disk2s4 instead of /Volumes/grees

1. dd: /dev/disk2s4: Resource busy

    1. info

            $ sudo dd if=CentOS-6.5-x86_64-bin-DVD1.iso of=/dev/disk2s4
            Password:
            dd: /dev/disk2s4: Resource busy

    1. check with `df`

            $ df -h
            Filesystem      Size   Used  Avail Capacity iused      ifree %iused  Mounted on
            /dev/disk1     233Gi  165Gi   67Gi    72% 2421155 4292546124    0%   /
            devfs          185Ki  185Ki    0Bi   100%     640          0  100%   /dev
            map -hosts       0Bi    0Bi    0Bi   100%       0          0  100%   /net
            map auto_home    0Bi    0Bi    0Bi   100%       0          0  100%   /home
            /dev/disk2s4    15Gi  2.6Gi   12Gi    18%       0          0  100%   /Volumes/gree2

    1. [fixed](https://raspberrypi.stackexchange.com/questions/9217/resource-busy-error-when-using-dd-to-copy-disk-img-to-sd-card)

            $ sudo diskutil unmount /Volumes/GREE2
            Volume GREE2 on disk2s4 unmounted

            $ df -h
            Filesystem      Size   Used  Avail Capacity iused      ifree %iused  Mounted on
            /dev/disk1     233Gi  165Gi   67Gi    72% 2422777 4292544502    0%   /
            devfs          187Ki  187Ki    0Bi   100%     646          0  100%   /dev
            map -hosts       0Bi    0Bi    0Bi   100%       0          0  100%   /net
            map auto_home    0Bi    0Bi    0Bi   100%       0          0  100%   /home

            $ sudo dd if=CentOS-6.5-x86_64-bin-DVD1.iso of=/dev/disk2s4
