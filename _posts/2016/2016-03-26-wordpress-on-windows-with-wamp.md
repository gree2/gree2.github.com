---
layout: post
title: "wordpress on windows with wamp"
description: ""
category: [setup]
tags: [wordpress, windows, wamp]
---
{% include JB/setup %}


### steps

1. download

    1. download [vc_redist.x64.exe](https://download.microsoft.com/download/9/3/F/93FCF1E7-E6A4-478B-96E7-D4B285925B00/vc_redist.x64.exe)

    1. download [wampserver](http://www.wampserver.com/en/#) direct url [wampserver3_x64_apache2.4.17_mysql5.7.9_php5.6.16_php7.0.0.exe](http://heanet.dl.sourceforge.net/project/wampserver/WampServer%203/WampServer%203.0.0/wampserver3_x64_apache2.4.17_mysql5.7.9_php5.6.16_php7.0.0.exe)

    1. download [wordpress-4.4.2.zip](https://wordpress.org/latest.zip)

1. install

    1. install `vc_redist.x64.exe`

    1. install `wampserver3`

1. setup wordpress

    1. use `wampserver`'s mysql console change `root`'s pasword, check [zhihu](http://www.zhihu.com/question/27334963)

            Enter password:
            Welcome to the MySQL monitor.  Commands end with ; or \g.
            Your MySQL connection id is 4
            Server version: 5.7.9 MySQL Community Server (GPL)

            Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

            Oracle is a registered trademark of Oracle Corporation and/or its
            affiliates. Other names may be trademarks of their respective
            owners.

            Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

            mysql> use mysql;
            Database changed
            mysql> update user set password=authentication_string('123456') where user='root';
            ERROR 1054 (42S22): Unknown column 'password' in 'field list'
            mysql> update user set authentication_string=password('123456') where user='root';
            Query OK, 1 row affected, 1 warning (0.00 sec)
            Rows matched: 1  Changed: 1  Warnings: 1

    1. create a database named `wordpress` use phpmyadmin

            # 1. login use user root and password 123456
            # 2. create database wordpress

    1. unzip `wordpress-4.4.2.zip`

            # 1. copy `wordpress` to `C:\wamp64\www`

    1. visit [http://localhost/wordpress/](http://localhost/wordpress/) follow the steps

### reference

1. [how-to-install-wordpress-on-your-windows-computer-using-wamp](http://www.wpbeginner.com/wp-tutorials/how-to-install-wordpress-on-your-windows-computer-using-wamp/)