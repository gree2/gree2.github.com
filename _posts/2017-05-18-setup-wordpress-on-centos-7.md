---
layout: post
title: "setup wordpress on centos 7"
description: ""
category: [setup]
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

1. setup apache

    1. change apache port

            $ sudo nano /etc/httpd/conf/httpd.conf
            Listen ip:8090

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
