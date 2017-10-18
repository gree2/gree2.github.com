---
layout: post
title: "centos 7.2 add a new disk drive"
description: ""
category: [linux]
tags: [ll, fdisk, df, mkfs, ext4, mount, fstab]
---
{% include JB/setup %}


### steps

1. list all disk drive

    1. use ll

            # ll /dev/disk/by-path
            lrwxrwxrwx. 1 root root  9 10 17 22:11 pci-0000:00:17.0-ata-1.0 -> ../../sda
            lrwxrwxrwx. 1 root root 10 10 17 22:11 pci-0000:00:17.0-ata-1.0-part1 -> ../../sda1
            lrwxrwxrwx. 1 root root 10 10 17 22:11 pci-0000:00:17.0-ata-1.0-part2 -> ../../sda2
            lrwxrwxrwx. 1 root root 10 10 17 22:11 pci-0000:00:17.0-ata-1.0-part3 -> ../../sda3
            lrwxrwxrwx. 1 root root 10 10 17 22:11 pci-0000:00:17.0-ata-1.0-part4 -> ../../sda4
            lrwxrwxrwx. 1 root root 10 10 17 22:11 pci-0000:00:17.0-ata-1.0-part5 -> ../../sda5
            lrwxrwxrwx. 1 root root  9 10 17 22:11 pci-0000:00:17.0-ata-2.0 -> ../../sdb
            lrwxrwxrwx. 1 root root 10 10 17 22:11 pci-0000:00:17.0-ata-2.0-part1 -> ../../sdb1
            lrwxrwxrwx. 1 root root  9 10 17 22:11 pci-0000:00:17.0-ata-3.0 -> ../../sr0
            lrwxrwxrwx. 1 root root  9 10 17 22:11 pci-0000:00:17.0-ata-4.0 -> ../../sdc
            lrwxrwxrwx. 1 root root 10 10 17 22:11 pci-0000:00:17.0-ata-4.0-part1 -> ../../sdc1

    1. use ls

            # ls /dev/sd*
            /dev/sda  /dev/sda1  /dev/sda2  /dev/sda3  /dev/sda4  /dev/sda5  /dev/sdb  /dev/sdb1  /dev/sdc  /dev/sdc1

1. format disk drive

    1. use fdisk

            # fdisk /dev/sdb
            ...
            Command (m for help): d <= delete a partition
            Command (m for help): n <= add a new partition
            Command action
               e extended
               p primary partition(1-4)
            p <= primary partition
            Partition number (1-4): 1
            ... <= use default value, hit enter key

            Command (m for help):p <= print the partition table
            ...
            Command (m for help):w <= write table to disk and exit

1. create a fs on disk partition

    1. use mkfs.ext4

            # mkfs.ext4 /dev/sdb1

1. mount fs

    1. use mount

            # mkdir /mnt/disk1
            # mount /dev/sdb1 /mnt/disk1
            # mount
            /dev/sda1 on /boot/efi type vfat (rw,relatime,fmask=0077,dmask=0077,codepage=437,iocharset=ascii,shortname=winnt,errors=remount-ro)
            /dev/sda2 on / type xfs (rw,relatime,seclabel,attr2,inode64,noquota)
            /dev/sda3 on /var/log type xfs (rw,relatime,seclabel,attr2,inode64,noquota)
            /dev/sdb1 on /mnt/disk1 type ext4 (rw,relatime,seclabel,data=ordered)
            /dev/sdc1 on /mnt/disk2 type ext4 (rw,relatime,seclabel,data=ordered)
            ...

1. auto mount config

    1. use /etc/fstab file

            # nano /etc/fstab
            ...
            /dev/sdb1                                 /mnt/disk1              ext4    defaults        0 0
            /dev/sdc1                                 /mnt/disk2              ext4    defaults        0 0

###