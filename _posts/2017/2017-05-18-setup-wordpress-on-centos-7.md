---
layout: post
title: "setup wordpress on centos 7"
description: ""
category: [devops]
tags: [wordpress, centos]
---
{% include JB/setup %}


### steps

1. setup lamp

            $ sudo yum -y install httpd mariadb-server mariadb php php-mysql php-gd php-xml

1. setup mariadb

    1. [start enable status](https://www.liquidweb.com/kb/how-to-install-mysql-mariadb-on-centos-7/)

            $ sudo systemctl start mariadb
            $ sudo systemctl enable mariadb
            Created symlink from /etc/systemd/system/multi-user.target.wants/mariadb.service to /usr/lib/systemd/system/mariadb.service.
            $ sudo systemctl status mariadb


    1. secure mysql

            $ mysql_secure_installation
            NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
                  SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

            In order to log into MariaDB to secure it, we'll need the current
            password for the root user.  If you've just installed MariaDB, and
            you haven't set the root password yet, the password will be blank,
            so you should just press enter here.

            Enter current password for root (enter for none):
            OK, successfully used password, moving on...

            Setting the root password ensures that nobody can log into the MariaDB
            root user without the proper authorisation.

            Set root password? [Y/n] y
            New password:
            Re-enter new password:
            Password updated successfully!
            Reloading privilege tables..
             ... Success!


            By default, a MariaDB installation has an anonymous user, allowing anyone
            to log into MariaDB without having to have a user account created for
            them.  This is intended only for testing, and to make the installation
            go a bit smoother.  You should remove them before moving into a
            production environment.

            Remove anonymous users? [Y/n] y
             ... Success!

            Normally, root should only be allowed to connect from 'localhost'.  This
            ensures that someone cannot guess at the root password from the network.

            Disallow root login remotely? [Y/n] n
             ... skipping.

            By default, MariaDB comes with a database named 'test' that anyone can
            access.  This is also intended only for testing, and should be removed
            before moving into a production environment.

            Remove test database and access to it? [Y/n] y
             - Dropping test database...
             ... Success!
             - Removing privileges on test database...
             ... Success!

            Reloading the privilege tables will ensure that all changes made so far
            will take effect immediately.

            Reload privilege tables now? [Y/n] y
             ... Success!

            Cleaning up...

            All done!  If you've completed all of the above steps, your MariaDB
            installation should now be secure.

            Thanks for using MariaDB!

    1. create database

            $ mysql -u root -p
            > create database wordpress;
            > create user wordpress@localhost identified by '************';
            > grant all privileges on wordpressdb.* to wordpress@localhost;
            > flush privileges;

1. setup apache

    1. change apache port

            $ sudo nano /etc/httpd/conf/httpd.conf
            Listen ip:8090
            Listen ip:8092

            $ sudo systemctl status httpd.service -l
            ● httpd.service - The Apache HTTP Server
               Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
               Active: failed (Result: exit-code) since 四 2017-05-18 13:31:08 CST; 2s ago
                 Docs: man:httpd(8)
                       man:apachectl(8)
              Process: 29766 ExecStop=/bin/kill -WINCH ${MAINPID} (code=exited, status=1/FAILURE)
              Process: 29763 ExecStart=/usr/sbin/httpd $OPTIONS -DFOREGROUND (code=exited, status=1/FAILURE)
             Main PID: 29763 (code=exited, status=1/FAILURE)

            5月 18 13:31:08 dc systemd[1]: Starting The Apache HTTP Server...
            5月 18 13:31:08 dc httpd[29763]: (13)Permission denied: AH00072: make_sock: could not bind to address 192.168.100.111:8090
            5月 18 13:31:08 dc httpd[29763]: no listening sockets available, shutting down
            5月 18 13:31:08 dc httpd[29763]: AH00015: Unable to open logs
            5月 18 13:31:08 dc systemd[1]: httpd.service: main process exited, code=exited, status=1/FAILURE
            5月 18 13:31:08 dc kill[29766]: kill: cannot find process ""
            5月 18 13:31:08 dc systemd[1]: httpd.service: control process exited, code=exited status=1
            5月 18 13:31:08 dc systemd[1]: Failed to start The Apache HTTP Server.
            5月 18 13:31:08 dc systemd[1]: Unit httpd.service entered failed state.
            5月 18 13:31:08 dc systemd[1]: httpd.service failed.

    1. [disable selinux](https://serverfault.com/questions/580277/having-trouble-starting-restarting-httpd-apache)

            $ sudo setenforce 0

1. setup firewalld

    1. env

            $ hostnamectl
               Static hostname: localhost.localdomain
            Transient hostname: dc
                     Icon name: computer-desktop
                       Chassis: desktop
                    Machine ID: b0b21aa9feba4b27964b91edc0de012b
                       Boot ID: acde97bca1734ce3ace7a91a9cb9b875
              Operating System: CentOS Linux 7 (Core)
                   CPE OS Name: cpe:/o:centos:centos:7
                        Kernel: Linux 3.10.0-514.2.2.el7.x86_64
                  Architecture: x86-64

            $ ip addr show | grep inet
            inet 127.0.0.1/8 scope host lo
            inet6 ::1/128 scope host
            inet 192.168.100.111/24 brd 192.168.100.255 scope global eno1
            inet6 fe80::c3aa:7b1a:6561:d450/64 scope link tentative dadfailed
            inet6 fe80::1789:916b:b94f:435b/64 scope link tentative dadfailed
            inet6 fe80::e018:9a79:5f15:c693/64 scope link tentative dadfailed
            inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
            inet 172.17.0.1/16 scope global docker0

    1. install firewalld

            $ sudo yum install firewalld -y

    1. stop and mask(not use anymore) iptables

            $ sudo systemctl status iptables
            $ sudo systemctl stop iptables
            $ sudo systemctl mask iptables
            Created symlink from /etc/systemd/system/iptables.service to /dev/null.

    1. enable and start firewalld

            $ sudo systemctl enable firewalld
            $ sudo systemctl restart firewalld
            $ sudo systemctl status firewalld
            ● firewalld.service - firewalld - dynamic firewall daemon
               Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
               Active: active (running) since...

    1. zones

            $ firewall-cmd --get-zones
            work drop internal external trusted home dmz public block

            $ firewall-cmd --get-default-zone
            public

            $ firewall-cmd --list-all-zones

    1. add port 8090

            $ firewall-cmd --get-active-zones
            public
                interfaces: eno1

            $ firewall-cmd --zone=public --add-port=8090/tcp --permanent
            success

            $ firewall-cmd --reload
            success
            $ firewall-cmd --list-all

1. setup wordpress

    1. [download](https://wordpress.org/download/)

    1. install

            $ unzip wordpress-4.7.5.zip
            $ rsync -avP wordpress /var/www/html/
            $ mkdir /var/www/html/wp-content/uploads
            $ sudo chown -R apache:apache /var/www/html/*

    1. config

            $ cd /var/www/html
            $ cp wp-config-sample.php wp-config.php
            $ nano wp-config.php
            define('DB_NAME', 'wordpress');
            define('DB_USER', 'wordpress');
            define('DB_PASSWORD', 'wordpress');

1. update wordpress

    1. backup

            $ cd /var/www/html
            $ mkdir worpress474
            $ mv *.php *.md *.txt wp-* wordpress474

    1. install and config

    1. restore themes

            $ cp -r wordpress474/wp-content/themes/roots-nextdatagov wp-content/themes/

### references

1. firewalld

    1. [https://www.tecmint.com/configure-firewalld-in-centos-7/](https://www.tecmint.com/configure-firewalld-in-centos-7/)

    1. [https://stackoverflow.com/questions/24729024/centos-7-open-firewall-port](https://stackoverflow.com/questions/24729024/centos-7-open-firewall-port)
