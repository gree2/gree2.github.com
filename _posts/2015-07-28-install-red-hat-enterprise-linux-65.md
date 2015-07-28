---
layout: post
title: "install red hat enterprise linux 6.5"
description: ""
category: [linux]
tags: [rhel, linux, ixgbe]
---
{% include JB/setup %}


### install

1. [disk partition](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s2-diskpartrecommend-x86.html)

1. [disk partition](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/3/html/Installation_Guide_x8664/s1-diskpartitioning.html)

### mount usb disk

1. commands

    1. mount

            # 1. find out the name of your block device
            $ fdisk -l

            # 2. create mount point
            $ mkdir /mnt/sdb1

            # 3. edit /etc/fstab
            $ pico /etc/fstab
            /dev/sdb1 /mnt/sdb1 vfat defaults 0 0

            # 4. 
            $ mount -a

            # or replace 3. 4. with this
            $ mount -t vfat /dev/sdb1 /mnt/sdb1

    2. umount

            $ umount /mnt/sdb1

1. references

    1. [howto mount usb drive in linux](http://linuxconfig.org/howto-mount-usb-drive-in-linux)

    1. [using the mount command](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Deployment_Guide/sect-Using_the_mount_Command-Mounting.html)

    1. [how to umount a usb drive](http://unix.stackexchange.com/questions/45820/how-to-umount-a-usb-drive)

### kernel-header and kernel-devel

1. download 1

    1. [download kernel-devel](ftp://rpmfind.net/linux/sourceforge/e/el/el6pantherproject/yum/kernel-alt-xhci/RPMS/x86_64/kernel-devel-2.6.32-431.29.2.el6.x86_64.rpm) from [here](http://rpmfind.net/linux/rpm2html/search.php?query=kernel-devel)

    1. [download kernel-header](ftp://rpmfind.net/linux/sourceforge/e/el/el6pantherproject/yum/kernel-alt-xhci/RPMS/x86_64/kernel-headers-2.6.32-431.29.2.el6.x86_64.rpm) from [here](http://rpmfind.net/linux/rpm2html/search.php?query=kernel-headers)

1. download 2

    1. [ftp.scientificlinux.org](http://ftp.scientificlinux.org/linux/scientific/6.4/x86_64/updates/security/)
            
            $ wget http://ftp.scientificlinux.org/linux/scientific/6.4/x86_64/updates/security/kernel-2.6.32-431.el6.x86_64.rpm
            
            $ wget http://ftp.scientificlinux.org/linux/scientific/6.4/x86_64/updates/security/kernel-debug-2.6.32-431.el6.x86_64.rpm
            
            $ wget http://ftp.scientificlinux.org/linux/scientific/6.4/x86_64/updates/security/kernel-debug-devel-2.6.32-431.el6.x86_64.rpm
            
            $ wget http://ftp.scientificlinux.org/linux/scientific/6.4/x86_64/updates/security/kernel-devel-2.6.32-431.el6.x86_64.rpm
            
            $ wget http://ftp.scientificlinux.org/linux/scientific/6.4/x86_64/updates/security/kernel-headers-2.6.32-431.el6.x86_64.rpm
            
            $ wget http://ftp.scientificlinux.org/linux/scientific/6.4/x86_64/updates/security/perf-2.6.32-431.el6.x86_64.rpm
            
            $ wget http://ftp.scientificlinux.org/linux/scientific/6.4/x86_64/updates/security/python-perf-2.6.32-431.el6.x86_64.rpm

1. install

            $ rpm -ivh *.rpm

            kernel-headers-2.6.32-431.el6.x86_64.rpm
            kernel-devel-2.6.32-431.el6.x86_64.rpm

1. [fixed](http://sourceforge.net/p/e1000/mailman/message/32659828/)

            $ uname -a
            Linux node2 2.6.32-431.el6.x86_64 #1 SMP Sun Nov 10 22:19:54 EST 2013 x86_64 x86_64 x86_64 GNU/Linux

            $ cat /etc/redhat-release
            Red Hat Enterprise Linux Server release 6.5 (Santiago)

            # --------------------------------------
            You don't have the right kernel headers.
            Look at the running kernel from uname:
            Linux gfs-5 2.6.32-431.20.3.el6.x86_64 #1 SMP Thu Jun 19 21:14:45 UTC
            and it's not the same as the headers you have installed:
            kernel-headers-2.6.32-431.20.5.el6.x86_64

1. you don't really want to download these rpms

            # check `mount dvd` section
            # all rmps you need is in dvd's `Packages` folder

### install compiler

1. [mount dvd](http://www.cyberciti.biz/faq/mounting-cdrom-in-linux/)

    1. find out your cd/dvd names in linux

            $ lsblk
            $ mkdir /mnt/dvd

    1. mount dvd/cdrom in linux

            $ mount -t iso9660 -o ro /dev/sr0 /mnt/dvd

1. [install gcc from redhat linux cdrom dvd](http://kevinzhaolinux.blogspot.com/2012/02/install-gcc-from-redhat-linux-cdrom-dvd.html)


    1. mount dvd

            $ mkdir /mnt/dvd
            $ mount /dev/sr0 /mnt/dvd

    1. go to packages directory

            $ cd /mnt/dvd/Packages

    1. install these packages as order

            $ cat rpm.sh
            rpm -ivh binutils-2.20.51.0.2-5.36.el6.x86_64.rpm
            rpm -ivh ppl-0.10.2-11.el6.x86_64.rpm
            rpm -ivh cloog-ppl-0.15.7-1.2.el6.x86_64.rpm
            rpm -ivh mpfr-2.4.1-6.el6.x86_64.rpm
            rpm -ivh cpp-4.4.7-4.el6.x86_64.rpm
            rpm -ivh glibc-devel-2.12-1.132.el6.x86_64.rpm
            rpm -ivh glibc-headers-2.12-1.132.el6.x86_64.rpm
            rpm -ivh kernel-devel-2.6.32-431.el6.x86_64.rpm
            rpm -ivh kernel-headers-2.6.32-431.el6.x86_64.rpm
            rpm -ivh gcc-4.4.7-4.el6.x86_64.rpm

            $ ./rmp.sh

    1. test gcc

            $ man gcc

### network interface driver

1. [downloads](http://sourceforge.net/projects/e1000/files/?source=typ_redirect)

1. install

            $ tar -zxf ixgbe-4.1.2.tar.gz
            $ cd ixgbe-4.1.2/src
            $ make install
            $ rmmod ixgbe; modprobe ixgbe

            $ modprobe ixgbe
            $ reboot

1. [/etc/hosts](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/3/html/System_Administration_Guide/s1-network-config-hosts.html)

1. [enable root login over ssh](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/V2V_Guide/Preperation_Before_the_P2V_Migration-Enable_Root_Login_over_SSH.html)

    1. edit `/etc/ssh/sshd_config`

            $ nano /etc/ssh/sshd_config
            # uncomment this line
            PermitRootLogin yes

    1. restart `ssh server`

            # service sshd restart

    1. test from my mac [ssh remote host identification has changed](http://stackoverflow.com/questions/20840012/ssh-remote-host-identification-has-changed)

            $ ssh root@node2
            @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
            @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
            @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
            IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
            Someone could be eavesdropping on you right now (man-in-the-middle attack)!
            It is also possible that a host key has just been changed.
            The fingerprint for the RSA key sent by the remote host is
            f7:e1:26:1f:f9:26:04:78:b9:3a:b9:46:a8:86:92:fe.
            Please contact your system administrator.
            Add correct host key in /Users/hqlgree2/.ssh/known_hosts to get rid of this message.
            Offending RSA key in /Users/hqlgree2/.ssh/known_hosts:12
            RSA host key for node2 has changed and you have requested strict checking.
            Host key verification failed.

            $ ssh-keygen -R node2
            # Host node2 found: line 12 type RSA
            /Users/hqlgree2/.ssh/known_hosts updated.
            Original contents retained as /Users/hqlgree2/.ssh/known_hosts.old
            $ ssh root@node2
            The authenticity of host 'node2 (192.168.120.152)' can't be established.
            RSA key fingerprint is f7:e1:26:1f:f9:26:04:78:b9:3a:b9:46:a8:86:92:fe.
            Are you sure you want to continue connecting (yes/no)? yes
            Warning: Permanently added 'node2' (RSA) to the list of known hosts.
            Warning: the RSA host key for 'node2' differs from the key for the IP address '192.168.120.152'
            Offending key for IP in /Users/hqlgree2/.ssh/known_hosts:9
            Are you sure you want to continue connecting (yes/no)? yes
            root@node2's password: 
            Last login: Tue Jul 28 16:20:30 2015
            -bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory

### configuration

1. /etc/hosts

            $ cat /etc/hosts
            127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
            ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
            192.168.120.155 node5
            192.168.120.154 node4
            192.168.120.153 node3
            192.168.120.152 node2
            192.168.120.151 node1

1. /etc/sysconfig/network-scripts/ifcfg-eth0

            $ cat /etc/sysconfig/network-scripts/ifcfg-eth0
            DEVICE=eth0
            TYPE=Ethernet
            UUID=df388ec6-d91d-4706-a01b-cdb5bc2edbd3
            ONBOOT=yes
            NM_CONTROLLED=yes
            BOOTPROTO=none
            HWADDR=14:DD:A9:55:EF:9D
            IPADDR=192.168.120.152
            PREFIX=24
            GATEWAY=192.168.120.1
            DNS1=192.168.10.220
            DNS2=192.168.10.221
            DEFROUTE=yes
            IPV4_FAILURE_FATAL=yes
            IPV6INIT=no
            NAME="System eth0"

1. /etc/ssh/sshd_config

            $ cat /etc/ssh/sshd_config
            #   $OpenBSD: sshd_config,v 1.80 2008/07/02 02:24:18 djm Exp $

            # This is the sshd server system-wide configuration file.  See
            # sshd_config(5) for more information.

            # This sshd was compiled with PATH=/usr/local/bin:/bin:/usr/bin

            # The strategy used for options in the default sshd_config shipped with
            # OpenSSH is to specify options with their default value where
            # possible, but leave them commented.  Uncommented options change a
            # default value.

            #Port 22
            #AddressFamily any
            #ListenAddress 0.0.0.0
            #ListenAddress ::

            # Disable legacy (protocol version 1) support in the server for new
            # installations. In future the default will change to require explicit
            # activation of protocol 1
            Protocol 2

            # HostKey for protocol version 1
            #HostKey /etc/ssh/ssh_host_key
            # HostKeys for protocol version 2
            #HostKey /etc/ssh/ssh_host_rsa_key
            #HostKey /etc/ssh/ssh_host_dsa_key

            # Lifetime and size of ephemeral version 1 server key
            #KeyRegenerationInterval 1h
            #ServerKeyBits 1024

            # Logging
            # obsoletes QuietMode and FascistLogging
            #SyslogFacility AUTH
            SyslogFacility AUTHPRIV
            #LogLevel INFO

            # Authentication:

            #LoginGraceTime 2m
            PermitRootLogin yes
            #StrictModes yes
            #MaxAuthTries 6
            #MaxSessions 10

            #RSAAuthentication yes
            #PubkeyAuthentication yes
            #AuthorizedKeysFile .ssh/authorized_keys
            #AuthorizedKeysCommand none
            #AuthorizedKeysCommandRunAs nobody

            # For this to work you will also need host keys in /etc/ssh/ssh_known_hosts
            #RhostsRSAAuthentication no
            # similar for protocol version 2
            #HostbasedAuthentication no
            # Change to yes if you don't trust ~/.ssh/known_hosts for
            # RhostsRSAAuthentication and HostbasedAuthentication
            #IgnoreUserKnownHosts no
            # Don't read the user's ~/.rhosts and ~/.shosts files
            #IgnoreRhosts yes

            # To disable tunneled clear text passwords, change to no here!
            #PasswordAuthentication yes
            #PermitEmptyPasswords no
            PasswordAuthentication yes

            # Change to no to disable s/key passwords
            #ChallengeResponseAuthentication yes
            ChallengeResponseAuthentication no

            # Kerberos options
            #KerberosAuthentication no
            #KerberosOrLocalPasswd yes
            #KerberosTicketCleanup yes
            #KerberosGetAFSToken no
            #KerberosUseKuserok yes

            # GSSAPI options
            #GSSAPIAuthentication no
            GSSAPIAuthentication yes
            #GSSAPICleanupCredentials yes
            GSSAPICleanupCredentials yes
            #GSSAPIStrictAcceptorCheck yes
            #GSSAPIKeyExchange no

            # Set this to 'yes' to enable PAM authentication, account processing, 
            # and session processing. If this is enabled, PAM authentication will 
            # be allowed through the ChallengeResponseAuthentication and
            # PasswordAuthentication.  Depending on your PAM configuration,
            # PAM authentication via ChallengeResponseAuthentication may bypass
            # the setting of "PermitRootLogin without-password".
            # If you just want the PAM account and session checks to run without
            # PAM authentication, then enable this but set PasswordAuthentication
            # and ChallengeResponseAuthentication to 'no'.
            #UsePAM no
            UsePAM yes

            # Accept locale-related environment variables
            AcceptEnv LANG LC_CTYPE LC_NUMERIC LC_TIME LC_COLLATE LC_MONETARY LC_MESSAGES
            AcceptEnv LC_PAPER LC_NAME LC_ADDRESS LC_TELEPHONE LC_MEASUREMENT
            AcceptEnv LC_IDENTIFICATION LC_ALL LANGUAGE
            AcceptEnv XMODIFIERS

            #AllowAgentForwarding yes
            #AllowTcpForwarding yes
            #GatewayPorts no
            #X11Forwarding no
            X11Forwarding yes
            #X11DisplayOffset 10
            #X11UseLocalhost yes
            #PrintMotd yes
            #PrintLastLog yes
            #TCPKeepAlive yes
            #UseLogin no
            #UsePrivilegeSeparation yes
            #PermitUserEnvironment no
            #Compression delayed
            #ClientAliveInterval 0
            #ClientAliveCountMax 3
            #ShowPatchLevel no
            #UseDNS yes
            #PidFile /var/run/sshd.pid
            #MaxStartups 10:30:100
            #PermitTunnel no
            #ChrootDirectory none

            # no default banner path
            #Banner none

            # override default of no subsystems
            Subsystem   sftp    /usr/libexec/openssh/sftp-server

            # Example of overriding settings on a per-user basis
            #Match User anoncvs
            #   X11Forwarding no
            #   AllowTcpForwarding no
            #   ForceCommand cvs server

### [disable firewall](http://www.cyberciti.biz/faq/disable-linux-firewall-under-centos-rhel-fedora/)

1. ipv4

            $ service iptables save
            $ service iptables stop
            $ chkconfig iptables off

1. ipv6

            $ service ip6tables save
            $ service ip6tables stop
            $ chkconfig ip6tables off

1. put in a script

            $ nano firewall.off
            service iptables save
            service iptables stop
            chkconfig iptables off
            service ip6tables save
            service ip6tables stop
            chkconfig ip6tables off

            $ chmod +x firewall.off
            $ ./firewall.off

            $ scp firewall.off root@node4:~/
            $ ssh node4
            $ ./firewall.off
            $ exit

### [setting file handles](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Tuning_and_Optimizing_Red_Hat_Enterprise_Linux_for_Oracle_9i_and_10g_Databases/chap-Oracle_9i_and_10g_Tuning_Guide-Setting_File_Handles.html)

1. file handles

    1. determine the maximum number of file handles for entire system

            $ cat /proc/sys/fs/file-max
            1608303

    1. determine the current usage of file handles

            $ cat /proc/sys/fs/file-nr
            5120    0   1608303

    1. change maximum number of file handles without reboot

            $ echo 640000

            $ sysctl -w fs.file-max=640000

    1. to make change permanent add or change `etc/sysctl.conf`

            $ nano /etc/sysctl.conf
            ...
            fs.file-max=640000
