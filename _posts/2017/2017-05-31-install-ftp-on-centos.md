---
layout: post
title: "install ftp on centos"
description: ""
category: [setup, linux]
tags: [ftp, centos, firewall]
---
{% include JB/setup %}


### steps

1. install ftp server

    1. install vsftpd

            $ sudo yum install vsftpd

    1. auto start when system boot

            $ sudo systemctl start vsftpd
            $ sudo systemctl enable vsftpd

    1. conf of firewall

            $ sudo firewall-cmd --zone=public --permanent --add-port=21/tcp
            $ sudo firewall-cmd --zone=public --permanent --add-service=ftp
            $ sudo firewall-cmd --reload

1. config ftp server

    1. /etc/vsftpd/vsftpd.con

            $ cd /etc/vsftpd/
            $ sudo cp vsftpd.conf vsftpd.conf.bak
            $ sudo nano vsftpd.conf
            anonymous_enable=NO             # disable  anonymous login
            local_enable=YES        # permit local logins
            write_enable=YES        # enable FTP commands which change the filesystem
            local_umask=022             # value of umask for file creation for local users
            dirmessage_enable=YES           # enable showing of messages when users first enter a new directory
            xferlog_enable=YES      # a log file will be maintained detailing uploads and downloads
            connect_from_port_20=YES        # use port 20 (ftp-data) on the server machine for PORT style connections
            xferlog_std_format=YES          # keep standard log file format
            listen=NO               # prevent vsftpd from running in standalone mode
            listen_ipv6=YES             # vsftpd will listen on an IPv6 socket instead of an IPv4 one
            pam_service_name=vsftpd         # name of the PAM service vsftpd will use
            userlist_enable=YES             # enable vsftpd to load a list of usernames
            tcp_wrappers=YES        # turn on tcp wrappers

            chroot_local_user=YES
            allow_writeable_chroot=YES

    1. securing ftp server with selinux [ftp_home_dir](https://bugzilla.redhat.com/show_bug.cgi?id=1097775)

            $ setsebool -P ftp_home_dir on
            Boolean ftp_home_dir is not defined
            $ semanage boolean -m ftpd_full_access --on

1. test ftp server

    1. add user

            $ useradd -m -c "ftpadmin" -s /bin/bash ftpadmin
            $ passwd ftpadmin

    1. config

            $ echo "ftpadmin" | tee -a /etc/vsftpd/user_list
            $ cat /etc/vsftpd/user_list

    1. testing

            $ ftp dc
            bash: ftp: command not found
            $ yum install ftp -y

1. config different ftp user home dir

    1. config

            $ nano vsftpd.conf
            # allow_writeable_chroot=YES
            user_sub_token=$USER
            local_root=/home/$USER/ftp

    1. local root dir

            $ mkdir /home/ftpadmin/ftp
            $ chown nobody:nobody /home/ftpadmin/ftp
            $ chmod a-w /home/ravi/ftp

    1. store files

            $ mdkir /home/ftpadmin/ftp/files/
            $ chown ftpadmin:ftpadmin /home/ftpadmin/ftp/files/
            $ chmod 0700 /home/ftpadmin/ftp/files/

    1. testing

            $ systemctl restart vsftpd
            $ ftp dc
            ftp> ls
            227 Entering Passive Mode (192,168,100,111,188,176).
            150 Here comes the directory listing.
            drwx------    2 1002     1002            6 May 31 07:55 files
            226 Directory send OK.

### references

1. [https://www.tecmint.com/install-ftp-server-in-centos-7/](https://www.tecmint.com/install-ftp-server-in-centos-7/)

1. [https://bugzilla.redhat.com/show_bug.cgi?id=1097775](https://bugzilla.redhat.com/show_bug.cgi?id=1097775)
