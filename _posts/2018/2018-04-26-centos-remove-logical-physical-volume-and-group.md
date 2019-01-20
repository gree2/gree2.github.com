---
layout: post
title: "centos remove logical physical volume and group"
description: ""
category: [linux]
tags: [centos, logical volume, volume group, physical volume, df, lvdisplay, lvremove, vgdisplay, vgremove, pvdisplay, pvremove]
---
{% include JB/setup %}


### steps

1. check fs

    1. list fs

            $ df -h
            文件系统                    容量  已用  可用 已用% 挂载点
            /dev/mapper/centos-root     250G  1.4G  249G    1% /
            devtmpfs                     16G     0   16G    0% /dev
            tmpfs                        16G     0   16G    0% /dev/shm
            tmpfs                        16G  8.5M   16G    1% /run
            tmpfs                        16G     0   16G    0% /sys/fs/cgroup
            /dev/sdb2                   294M  108M  187M   37% /boot
            /dev/sdb1                   200M  9.5M  191M    5% /boot/efi
            /dev/mapper/centos-var_log  181G   36M  181G    1% /var/log
            /dev/mapper/centos-home     1.4T   33M  1.4T    1% /home
            tmpfs                       3.2G     0  3.2G    0% /run/user/0

            /dev/mapper/docker-docker--pool_tmeta
            /dev/mapper/docker-docker--pool_tdata
            /dev/mapper/docker-docker--pool

    1. umount

            $ umount /var/lib/docker

1. remove logical volume

    1. list

            $ lvdisplay
              --- Logical volume ---
              LV Name                docker-pool
              VG Name                docker
              LV UUID                0bD9D6-sKc8-52fz-oigL-S162-cqvY-k4MSKG
              LV Write Access        read/write
              LV Creation host, time poc1, 2017-12-12 23:18:57 +0800
              LV Pool metadata       docker-pool_tmeta
              LV Pool data           docker-pool_tdata
              LV Status              available
              # open                 0
              LV Size                55.30 GiB
              Allocated pool data    50.52%
              Allocated metadata     5.34%
              Current LE             14158
              Segments               1
              Allocation             inherit
              Read ahead sectors     auto
              - currently set to     256
              Block device           253:4

              --- Logical volume ---
              LV Path                /dev/centos/root
              LV Name                root
              VG Name                centos
              LV UUID                3C5niV-07n6-QDwO-xYLi-CU1n-wuqe-z4pH6b
              LV Write Access        read/write
              LV Creation host, time poc1, 2018-04-21 11:04:53 +0800
              LV Status              available
              # open                 1
              LV Size                250.00 GiB
              Current LE             64000
              Segments               1
              Allocation             inherit
              Read ahead sectors     auto
              - currently set to     256
              Block device           253:0

              --- Logical volume ---
              LV Path                /dev/centos/swap
              LV Name                swap
              VG Name                centos
              LV UUID                VaHYRf-JpcO-l2ve-fpMq-lGjL-sSa9-waTqu5
              LV Write Access        read/write
              LV Creation host, time poc1, 2018-04-21 11:04:55 +0800
              LV Status              available
              # open                 2
              LV Size                32.00 GiB
              Current LE             8192
              Segments               1
              Allocation             inherit
              Read ahead sectors     auto
              - currently set to     256
              Block device           253:1

              --- Logical volume ---
              LV Path                /dev/centos/var_log
              LV Name                var_log
              VG Name                centos
              LV UUID                xrgvm6-UXae-g7eH-Mrjf-UGE0-sjwl-yxJTBR
              LV Write Access        read/write
              LV Creation host, time poc1, 2018-04-21 11:04:55 +0800
              LV Status              available
              # open                 1
              LV Size                180.52 GiB
              Current LE             46213
              Segments               1
              Allocation             inherit
              Read ahead sectors     auto
              - currently set to     256
              Block device           253:5

              --- Logical volume ---
              LV Path                /dev/centos/home
              LV Name                home
              VG Name                centos
              LV UUID                5zvoiW-sFFo-vSZp-NFtZ-cu96-khuk-DugJUG
              LV Write Access        read/write
              LV Creation host, time poc1, 2018-04-21 11:04:56 +0800
              LV Status              available
              # open                 1
              LV Size                1.37 TiB
              Current LE             358400
              Segments               1
              Allocation             inherit
              Read ahead sectors     auto
              - currently set to     256
              Block device           253:6

    1. remove

            $ lvremove docker
            Do you really want to remove active logical volume docker-pool? [y/n]: y
              /etc/lvm/profile/docker--docker-pool-extend.profile: stat failed: 没有那个文件或目录
              Logical volume "docker-pool" successfully removed

1. remove volume group

    1. list

            $ vgdisplay
              --- Volume group ---
              VG Name               docker
              System ID
              Format                lvm2
              Metadata Areas        1
              Metadata Sequence No  15
              VG Access             read/write
              VG Status             resizable
              MAX LV                0
              Cur LV                1
              Open LV               0
              Max PV                0
              Cur PV                1
              Act PV                1
              VG Size               238.47 GiB
              PE Size               4.00 MiB
              Total PE              61049
              Alloc PE / Size       14282 / 55.79 GiB
              Free  PE / Size       46767 / 182.68 GiB
              VG UUID               33sfpV-MrS3-ydoC-2mn1-TwM8-2w1Z-8XHSyP

              --- Volume group ---
              VG Name               centos
              System ID
              Format                lvm2
              Metadata Areas        1
              Metadata Sequence No  5
              VG Access             read/write
              VG Status             resizable
              MAX LV                0
              Cur LV                4
              Open LV               4
              Max PV                0
              Cur PV                1
              Act PV                1
              VG Size               1.82 TiB
              PE Size               4.00 MiB
              Total PE              476806
              Alloc PE / Size       476805 / 1.82 TiB
              Free  PE / Size       1 / 4.00 MiB
              VG UUID               pH7wdj-2Jjj-kI7r-X8WT-cjvJ-z56n-7KGu1V

    1. remove volume group

            $ vgremove docker

1. remove physical volume

    1. list

            $ pvdisplay
              --- Physical volume ---
              PV Name               /dev/sda1
              VG Name               docker
              PV Size               238.47 GiB / not usable 1.34 MiB
              Allocatable           yes
              PE Size               4.00 MiB
              Total PE              61049
              Free PE               46767
              Allocated PE          14282
              PV UUID               sgDwvX-eqDn-ZzOy-byP3-l00Z-ofFz-t5ZN9s

              --- Physical volume ---
              PV Name               /dev/sdb3
              VG Name               centos
              PV Size               1.82 TiB / not usable 4.00 MiB
              Allocatable           yes
              PE Size               4.00 MiB
              Total PE              476806
              Free PE               1
              Allocated PE          476805
              PV UUID               KEXnWk-oGiN-yaxL-gzMW-oMjl-qylf-edidJt

    1. remove

            $ pvremove /dev/sda1
