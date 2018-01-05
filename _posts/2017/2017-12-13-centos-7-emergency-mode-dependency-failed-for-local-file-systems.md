---
layout: post
title: "centos 7 emergency mode dependency failed for local file systems"
description: ""
category: [linux]
tags: [centos, fdisk, emergency, mode, dependency, journalctl]
---
{% include JB/setup %}


### create partition and failed into emergency mode

1. my home folder

    1. original disk layout

            $ fdisk -l

            WARNING: fdisk GPT support is currently new, and therefore in an experimental phase. Use at your own discretion.

            disk /dev/sda：120.0 GB, 120034123776 bytes，234441648 cylinders
            Units = cylinders of 1 * 512 = 512 bytes
            Sector size(logical/physical)：512 bytes / 512 bytes
            I/O size(minimum/optimal)：512 bytes / 512 bytes
            Disk identifier：gpt


            #         Start          End    Size  Type            Name
             1         2048       411647    200M  EFI System      EFI System Partition
             2       411648      2508799      1G  Microsoft basic
             3      2508800    234440703  110.6G  Linux LVM
            WARNING: fdisk GPT support is currently new, and therefore in an experimental phase. Use at your own discretion.

            Disk /dev/sdb：2000.4 GB, 2000398934016 bytes，3907029168 个cylinders
            Units = cylinders  of 1 * 512 = 512 bytes
            Sector size(logical/physical)：512 bytes / 4096 bytes
            I/O size(minimum/optimal)：4096 bytes / 4096 bytes
            Disk identifier：gpt


            #         Start          End    Size  Type            Name
             1         2048   3907029134    1.8T  Linux filesyste

            Disk /dev/mapper/cl-root：53.7 GB, 53687091200 bytes，104857600 cylinders
            Units = cylinders  of 1 * 512 = 512 bytes
            Sector (logical/physical)：512 bytes / 512 bytes
            I/O size(minimum/optimal)：512 bytes / 512 bytes


            Disk /dev/mapper/cl-swap：16.7 GB, 16710107136 bytes，32636928 cylinders
            Units = cylinders  of 1 * 512 = 512 bytes
            Sector (logical/physical)：512 bytes / 512 bytes
            I/O size(minimum/optimal)：512 bytes / 512 bytes


            Disk /dev/mapper/cl-home：2048.7 GB, 2048737148928 bytes，4001439744 cylinders
            Units = cylinders  of 1 * 512 = 512 bytes
            Sector (logical/physical)：512 bytes / 4096 bytes
            I/O size(minimum/optimal)：4096 bytes / 4096 bytes

    1. create 2 partitions on /dev/sdb

            $ fdisk /dev/sdb

            $ fdisk -l

            Disk /dev/sdb：2000.4 GB, 2000398934016 bytes，3907029168 个cylinders
            Units = cylinders  of 1 * 512 = 512 bytes
            Sector size(logical/physical)：512 bytes / 4096 bytes
            I/O size(minimum/optimal)：4096 bytes / 4096 bytes
            Disk identifier：gpt


            #         Start          End    Size   Type            Name
             1         xxxx   xxxxxxxxxx    300GB  Linux filesyste
             2         xxxx   xxxxxxxxxx    300GB  Linux filesyste

    1. then reboot and failed... and enter emergency mode

1. emergency mode

    1. info

            welcome to emergency mode! after logging in
            type journalctl -xb to view system logs
            systemctl reboot to reboot
            systemctl default or ^D to try againt to boot into default mode

    1. check journal

            $ journalctl -xb
            ...
            Dependency failded for /home
            ...
            Dependency failded for Local File Systems
            ...

    1. check disk info

            $ fdisk -lu

            /dev/mapper/cl-root
            /dev/mapper/cl-swap
            # /dev/mapper/cl-home <-- missing

    1. check pvs

            $ pvs
            WARMING: Device /dev/sdb1 has size of 629145600 sectors which is smaller than
            corresponding PV size of 3907026944 sectors. Was device resized?
            One or more devices used as PVs in VG cl have changed sizes.
            PV        VG Fmt  Attr PSize   PFree
            /dev/sda3 cl lvm2 a--  110.59g 4.00m
            /dev/sdb1 cl lvm2 a--  1.82t   4.00m

    1. check vgs

            $ vgs
            WARMING: Device /dev/sdb1 has size of 629145600 sectors which is smaller than
            corresponding PV size of 3907026944 sectors. Was device resized?
            One or more devices used as PVs in VG cl have changed sizes.
            VG #PV #LV #SN Attr   VSize VFree
            cl   2   3   0 wz--n- 1.93t 4.00m

    1. check lvs

            $ lvs
            WARMING: Device /dev/sdb1 has size of 629145600 sectors which is smaller than
            corresponding PV size of 3907026944 sectors. Was device resized?
            One or more devices used as PVs in VG cl have changed sizes.
            LV   VG Attr       LSize  Pool Origin Data% Meta% Move Log Cpy%Sync Convert
            home cl -wi-s----- 1.86t
            root cl -wi-ao---- 50.00g
            swap cl -wi-ao---- 15.56g

    1. check fstab

            $ cat /etc/fstab
            /dev/mapper/cl-root                       /         xfs  defaults                   0 0
            UUID=c7e352df-0343-4c59-8972-d831510a0b72 /boot     xfs  defaults                   0 0
            UUID=13A6-95A2                            /boot/efi vfat umask=0077,shortname=winnt 0 0
            /dev/mapper/cl-home                       /hom      xfs  defaults                   0 0
            /dev/mapper/cl-swap                       swap      swap defaults                   0 0

### fixed

1. steps

    1. delete all partition on /dev/sdb

    1. then reboot

    1. that all
