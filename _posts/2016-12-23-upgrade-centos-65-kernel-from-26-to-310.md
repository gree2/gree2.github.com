---
layout: post
title: "upgrade centos 6.5 kernel from 2.6 to 3.10"
description: ""
category: [setup]
tags: [centos, 6.5, kernel, 2.6, 3.10, epel, elrepo]
---
{% include JB/setup %}


### steps

1. upgrade kernel

    1. install package `epel-release-6-8.noarch`

            [gree2@test ~]$ sudo rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
            [sudo] password for test:
            Retrieving http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
            Preparing...                ########################################### [100%]
                package epel-release-6-8.noarch is already installed

    1. import public key
            
            [gree2@test ~]$ sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org

    1. install elrepo
            
            [gree2@test ~]$ sudo rpm -Uvh http://www.elrepo.org/elrepo-release-6-6.el6.elrepo.noarch.rpm
            Retrieving http://www.elrepo.org/elrepo-release-6-6.el6.elrepo.noarch.rpm
            Preparing...                ########################################### [100%]
               1:elrepo-release         ########################################### [100%]

    1. install kernel

            [gree2@test ~]$ sudo yum --enablerepo=elrepo-kernel install kernel-lt –y
            已加载插件：fastestmirror, refresh-packagekit, security
            设置安装进程
            Loading mirror speeds from cached hostfile
            epel/metalink | 6.5 kB     00:00
             * base: mirrors.aliyun.com
             * elrepo: mirrors.tuna.tsinghua.edu.cn
             * elrepo-kernel: mirrors.tuna.tsinghua.edu.cn
             * epel: mirrors.tuna.tsinghua.edu.cn
             * extras: mirrors.aliyun.com
             * updates: mirrors.aliyun.com
            WANdisco-git                                                 |  951 B     00:00
            base                                                         | 3.7 kB     00:00
            elrepo                                                       | 2.9 kB     00:00
            elrepo/primary_db                                            | 727 kB     00:01
            elrepo-kernel                                                | 2.9 kB     00:00
            elrepo-kernel/primary_db                                     |  20 kB     00:00
            epel                                                         | 4.3 kB     00:00
            epel/primary_db                                              | 5.9 MB     02:03
            extras                                                       | 3.4 kB     00:00
            gitlab-ce                                                    | 2.9 kB     00:00
            gitlab-ce/primary_db                                         | 1.1 MB     00:03
            updates                                                      | 3.4 kB     00:00
            http://mirrors.aliyun.com/centos/6/updates/x86_64/repodata/db9ad4c9c72226a12da21d3020b392a506ed5bf8bc05e69a3a80392b579fc790-primary.sqlite.bz2: [Errno 12] Timeout on http://106.2.184.228:9999/mirrors.aliyun.com/centos/6/updates/x86_64/repodata/db9ad4c9c72226a12da21d3020b392a506ed5bf8bc05e69a3a80392b579fc790-primary.sqlite.bz2: (28, 'connect() timed out!')
            尝试其他镜像。
            http://mirrors.aliyuncs.com/centos/6/updates/x86_64/repodata/db9ad4c9c72226a12da21d3020b392a506ed5bf8bc05e69a3a80392b579fc790-primary.sqlite.bz2: [Errno 14] PYCURL ERROR 7 - "couldn't connect to host"
            尝试其他镜像。
            解决依赖关系
            --> 执行事务检查
            ---> Package kernel-lt.x86_64 0:3.10.104-1.el6.elrepo will be 安装
            --> 完成依赖关系计算

            依赖关系解决

            ===============================================================================
             软件包         架构        版本                       仓库               大小
            ===============================================================================
            正在安装:
             kernel-lt     x86_64     3.10.104-1.el6.elrepo     elrepo-kernel     33 M

            事务概要
            ===============================================================================
            Install       1 Package(s)

            总下载量：33 M
            Installed size: 154 M
            确定吗？[y/N]：y
            下载软件包：
            kernel-lt-3.10.104-1.el6.elrepo.x86_64.rpm                   |  33 MB     03:21
            运行 rpm_check_debug
            执行事务测试
            事务测试成功
            执行事务
            Warning: RPMDB altered outside of yum.
              正在安装   : kernel-lt-3.10.104-1.el6.elrepo.x86_64         1/1
              Verifying  : kernel-lt-3.10.104-1.el6.elrepo.x86_64        1/1

            已安装:
              kernel-lt.x86_64 0:3.10.104-1.el6.elrepo

            完毕！

1. use new kernel

    1. edit boot menu set `default=0` from `default=1`

            [gree2@test ~]$ sudo cat /etc/grub.conf
            [sudo] password for test:
            # grub.conf generated by anaconda
            #
            # Note that you do not have to rerun grub after making changes to this file
            # NOTICE:  You do not have a /boot partition.  This means that
            #          all kernel and initrd paths are relative to /, eg.
            #          root (hd0,2)
            #          kernel /boot/vmlinuz-version ro root=/dev/sda3
            #          initrd /boot/initrd-[generic-]version.img
            #boot=/dev/sda1
            device (hd0) HD(1,800,fa000,42bf98ee-ca79-434f-8a7d-d4973a223a64)
            default=0
            timeout=5
            splashimage=(hd0,2)/boot/grub/splash.xpm.gz
            hiddenmenu
            title CentOS (3.10.104-1.el6.elrepo.x86_64)
                root (hd0,2)
                kernel /boot/vmlinuz-3.10.104-1.el6.elrepo.x86_64 ro root=UUID=c37a3cb8-201b-4a97-b5d8-350b44db13fe rd_NO_LUKS rd_NO_LVM LANG=en_US.UTF-8 rd_NO_MD SYSFONT=latarcyrheb-sun16 crashkernel=128M  KEYBOARDTYPE=pc KEYTABLE=us rd_NO_DM rhgb quiet
                initrd /boot/initramfs-3.10.104-1.el6.elrepo.x86_64.img
            title CentOS (2.6.32-642.6.1.el6.x86_64)
                root (hd0,2)
                kernel /boot/vmlinuz-2.6.32-642.6.1.el6.x86_64 ro root=UUID=c37a3cb8-201b-4a97-b5d8-350b44db13fe rd_NO_LUKS rd_NO_LVM LANG=en_US.UTF-8 rd_NO_MD SYSFONT=latarcyrheb-sun16 crashkernel=128M  KEYBOARDTYPE=pc KEYTABLE=us rd_NO_DM rhgb quiet
                initrd /boot/initramfs-2.6.32-642.6.1.el6.x86_64.img
            title CentOS (2.6.32-431.el6.x86_64)
                root (hd0,2)
                kernel /boot/vmlinuz-2.6.32-431.el6.x86_64 ro root=UUID=c37a3cb8-201b-4a97-b5d8-350b44db13fe rd_NO_LUKS rd_NO_LVM LANG=en_US.UTF-8 rd_NO_MD SYSFONT=latarcyrheb-sun16 crashkernel=128M  KEYBOARDTYPE=pc KEYTABLE=us rd_NO_DM rhgb quiet
                initrd /boot/initramfs-2.6.32-431.el6.x86_64.img

    1. check kernel version before reboot
            
            [gree2@test ~]$ uname -r
            2.6.32-642.6.1.el6.x86_64

    1. reboot
            
            [gree2@test ~]$ sudo reboot

    1. check kernel version after reboot
            
            [gree2@test ~]$ uname -r
            3.10.104-1.el6.elrepo.x86_64

### references

1. [http://elrepo.org/tiki/tiki-index.php](http://elrepo.org/tiki/tiki-index.php)