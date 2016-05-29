---
layout: post
title: "farewell windows welcome ubuntu"
description: ""
category: [linux]
tags: [install, partition, ubuntu]
---
{% include JB/setup %}


### install ubuntu 16.04

1. plan

    1. disk

            ssd => boot root swap etc.
            hd  => home etc.

    1. first

            install os on ssh by default fs setup
            so my home folder is on ssd

    1. second

            partition hd
            and move home from ssd to hd

1. installation

            blablabla...

### partition disk

1. find my `ssd` and `hd`

            $ sudo fdisk -lu

            ...

            Disk /dev/sda: 232.9 GiB, 250059350016 bytes, 488397168 sectors
            Units: sectors of 1 * 512 = 512 bytes
            Sector size (logical/physical): 512 bytes / 512 bytes
            I/O size (minimum/optimal): 512 bytes / 512 bytes
            Disklabel type: dos
            Disk identifier: 0xb20e1ae8

            Device     Boot   Start       End   Sectors   Size Id Type
            /dev/sda1  *       2048    999423    997376   487M 83 Linux
            /dev/sda2       1001470 488396799 487395330 232.4G  5 Extended
            /dev/sda5       1001472 488396799 487395328 232.4G 8e Linux LVM


            Disk /dev/sdb: 465.8 GiB, 500107862016 bytes, 976773168 sectors
            Units: sectors of 1 * 512 = 512 bytes
            Sector size (logical/physical): 512 bytes / 4096 bytes
            I/O size (minimum/optimal): 4096 bytes / 4096 bytes
            Disklabel type: gpt
            Disk identifier: C95CAD6D-F26D-425A-8E91-1736A0C5B078

            Device         Start       End   Sectors   Size Type
            /dev/sdb1       2048 419432447 419430400   200G Microsoft basic data
            /dev/sdb2  419432448 976771071 557338624 265.8G Microsoft basic data

            ...

1. partition
    
    1. delete partitions on `/dev/sdb`

            $ sudo fdisk /dev/sdb

            Welcome to fdisk (util-linux 2.27.1).
            Changes will remain in memory only, until you decide to write them.
            Be careful before using the write command.


            Command (m for help): m

            Help:

              Generic
               d   delete a partition
               F   list free unpartitioned space
               l   list known partition types
               n   add a new partition
               p   print the partition table
               t   change a partition type
               v   verify the partition table
               i   print information about a partition

              Misc
               m   print this menu
               x   extra functionality (experts only)

              Script
               I   load disk layout from sfdisk script file
               O   dump disk layout to sfdisk script file

              Save & Exit
               w   write table to disk and exit
               q   quit without saving changes

              Create a new label
               g   create a new empty GPT partition table
               G   create a new empty SGI (IRIX) partition table
               o   create a new empty DOS partition table
               s   create a new empty Sun partition table


            Command (m for help): d
            Partition number (1,2, default 2): 2

            Partition 2 has been deleted.

            Command (m for help): d
            Selected partition 1
            Partition 1 has been deleted.

    1. create a new partition

            Command (m for help): F
            Unpartitioned space /dev/sdb: 465.8 GiB, 500106796544 bytes, 976771087 sectors
            Units: sectors of 1 * 512 = 512 bytes
            Sector size (logical/physical): 512 bytes / 4096 bytes

            Start       End   Sectors   Size
             2048 976773134 976771087 465.8G

            Command (m for help): n
            Partition number (1-128, default 1): 1
            First sector (34-976773134, default 2048): 
            Last sector, +sectors or +size{K,M,G,T,P} (2048-976773134, default 976773134): 

            Created a new partition 1 of type 'Linux filesystem' and of size 465.8 GiB.

            Command (m for help): p
            Disk /dev/sdb: 465.8 GiB, 500107862016 bytes, 976773168 sectors
            Units: sectors of 1 * 512 = 512 bytes
            Sector size (logical/physical): 512 bytes / 4096 bytes
            I/O size (minimum/optimal): 4096 bytes / 4096 bytes
            Disklabel type: gpt
            Disk identifier: C95CAD6D-F26D-425A-8E91-1736A0C5B078

            Device     Start       End   Sectors   Size Type
            /dev/sdb1   2048 976773134 976771087 465.8G Linux filesystem

            Command (m for help): w
            The partition table has been altered.
            Calling ioctl() to re-read partition table.
            Syncing disks.

1. check partition result

            $ sudo fdisk -lu

            ...

            Disk /dev/sdb: 465.8 GiB, 500107862016 bytes, 976773168 sectors
            Units: sectors of 1 * 512 = 512 bytes
            Sector size (logical/physical): 512 bytes / 4096 bytes
            I/O size (minimum/optimal): 4096 bytes / 4096 bytes
            Disklabel type: gpt
            Disk identifier: C95CAD6D-F26D-425A-8E91-1736A0C5B078

            Device     Start       End   Sectors   Size Type
            /dev/sdb1   2048 976773134 976771087 465.8G Linux filesystem

            ...

1. mount to somewhere

    1. check all mounted fs

            $ sudo df -l
            Filesystem                  1K-blocks    Used Available Use% Mounted on
            udev                          8031300       0   8031300   0% /dev
            tmpfs                         1610240    9692   1600548   1% /run
            /dev/mapper/ubuntu--vg-root 223512140 3745684 208389612   2% /
            tmpfs                         8051188     208   8050980   1% /dev/shm
            tmpfs                            5120       4      5116   1% /run/lock
            tmpfs                         8051188       0   8051188   0% /sys/fs/cgroup
            /dev/sda1                      482922   58155    399833  13% /boot
            tmpfs                         1610240     104   1610136   1% /run/user/1000

    1. mount to `/mnt/dat`

            $ sudo mkdir /mnt/dat
            $ sudo mount -t ext4 /dev/sdb /mnt/dat

    1. check again

            $ sudo df -l
            Filesystem                  1K-blocks    Used Available Use% Mounted on
            udev                          8031300       0   8031300   0% /dev
            tmpfs                         1610240    9692   1600548   1% /run
            /dev/mapper/ubuntu--vg-root 223512140 3745692 208389604   2% /
            tmpfs                         8051188     208   8050980   1% /dev/shm
            tmpfs                            5120       4      5116   1% /run/lock
            tmpfs                         8051188       0   8051188   0% /sys/fs/cgroup
            /dev/sda1                      482922   58155    399833  13% /boot
            tmpfs                         1610240     104   1610136   1% /run/user/1000
            /dev/sdb                    480590568   71592 456083264   1% /mnt/dat

1. make it usable from os boot

    1. check uuid

            $ sudo blkid /dev/sdb
            /dev/sdb: UUID="0115d114-426b-4bcd-8956-9990bf87da01" TYPE="ext4"

    1. write to `/etc/fstab`

            $ cat /etc/fstab 
            # /etc/fstab: static file system information.
            #
            # Use 'blkid' to print the universally unique identifier for a
            # device; this may be used with UUID= as a more robust way to name devices
            # that works even if disks are added and removed. See fstab(5).
            #
            # <file system> <mount point>   <type>  <options>       <dump>  <pass>
            /dev/mapper/ubuntu--vg-root               /        ext4 errors=remount-ro 0       1
            # /boot was on /dev/sda1 during installation
            UUID=56d0880d-b2f4-425f-8b81-9ad3aaf8f2b4 /boot    ext2 defaults          0       2
            /dev/mapper/ubuntu--vg-swap_1             none     swap sw                0       0
            # /mnt/dat was on /dev/sdb
            UUID=0115d114-426b-4bcd-8956-9990bf87da01 /mnt/dat ext4 defaults          0       2

### move home from ssd to hd

1. [move-home-folder-to-second-drive](http://askubuntu.com/questions/21321/move-home-folder-to-second-drive)

    1. temporarily mount the new partition:

            # note that we already mount `/dev/sdb` to /mnt/dat

    1. copy `home` to the new location:
            
            $ sudo rsync -avx /home/ /mnt/dat

    1. mount the new partition as `home` with

            $ sudo mount /dev/sdb /home

    1. unmount the new home first!
            
            to make sure all data are present
            easiest is to delete the old /home at this point
            (you could do this later but then
            you will have to boot a live system to see the old home)

            $ sudo umount /home  
            
            # deletes the old home
            $ rm -rf /home/*

    1. make `home` permanent

            # edit `/etc/fstab`

            # change this
            # /mnt/dat was on /dev/sdb
            UUID=0115d114-426b-4bcd-8956-9990bf87da01 /mnt/dat ext4 defaults          0       2

            # to this
            # /home was on /dev/sdb
            UUID=0115d114-426b-4bcd-8956-9990bf87da01 /home    ext4 defaults          0       2

    1. reboot

            after a reboot
            your /home resides on the new drive having plenty of space

### software

1. input

    1. sogou

    1. [download](http://pinyin.sogou.com/linux/?r=pinyin)

    1. double click to install

1. wechat

    1. [electronic-wechat](https://github.com/geeeeeeeeek/electronic-wechat)

    1. [download](http://o763lpzct.bkt.clouddn.com/linux-x64.tar.gz)

    1. install

            $ tar zxvf linux-x64.tar.gz
            $ mv electronic* wechat
            $ cd wechat
            $ ./electronic-wechat
