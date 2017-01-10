---
layout: post
title: "setup wordpress on ubuntu"
description: ""
category: [setup]
tags: [wordpress, lamp, apache, mysql, php]
---
{% include JB/setup %}


### steps

1. setup lamp

    1. apache

            $ sudo apt-get insatll apache2

    1. mysql

            $ sudo apt-get install mysql-server php5-mysql

            $ sudo mysql_install_db

            $ sudo mysql_secure_installation

    1. php

            $ sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt

            $ sudo pico /etc/apache2/mods-enabled/dir.conf
            <IfModule mod_dir.c>
                DirectoryIndex index.html index.cgi index.pl index.php index.html index.htm
            </IfModule>

            # move index.php to first position

            $ sudo service apache2 restart

    1. php modules

            # 1. search
            $ apt-cache search php5-

            ...
            result
            ...

            # 2. more info
            $ apt-cache show php5-cli

            # 3. install
            $ sudo apt-get install php5-cli

    1. test php processing

            # 1. php code
            $ sudo pico /var/www/html/info.php
            <?php phpinfo(); ?>

            # 2. visit
            http://localhost/info.php

1. create database and user

    1. create database
    
            $ mysql -u root -p
            mysql> create database wordpress;

    1. create user

            mysql> create user wordpressuser@localhost identified by 'node';

    1. granting

            mysql> grant all privileges on wordpress.* to wordpressuser@localhost;
            mysql> flush privileges;
            mysql exit;

1. setup wordpress

    1. download wordpress

            $ cd
            $ wget http://wordpress.org/latest.tar.gz
            $ tar zxvf latest.tar.gz
            
    1. config wordpress

            $ cd wordpress
            $ cp wp-config-sample.php wp-config.php
            $ pico wp-config.php

            # settings for database
            define('DB_NAME', 'wordpress');
            define('DB_USER', 'wordpressuser');
            define('DB_PASSWORD', 'node');

    1. copy files to document root

            # 1. copy files
            $ sudo rsync -avP ~/wordpress /var/www/html/

            # 2. assign ownership
            $ cd /var/www/html
            $ sudo chown -R node:www-data *

            # 3. forder for uploads
            $ mkdir /var/www/html/wp-content/uploads

            # 4. assign ownership
            $ sudo chown -R :www-data /var/www/html/wp-content/uploads

    1. complete installation through the web-ui

            # 1. visit
            http://localhost

            # 2. fill info
            site title
            username
            password
            email

            click `install wordpress`

            # 3. log in

    1. optional config pretty permalinks for wordpress

            # 1. url rewrites
            $ sudo pico /etc/apache2/site-available/000-default.conf

            <VirtualHost *:80>
                ServerAdmin webmaster@localhost
                DocumentRoot /var/www/html
                ServerName 127.0.0.1
                <Directory /var/www/html/>
                    AllowOverride All
                </Directory>
                . . .

            # 2. enable the rewrite module
            $ sudo a2enmod rewrite

            # 3. restart
            $ sudo service apache2 restart

            # 4. create `.htaccess` file
            $ touch /var/www/html/.htaccess
            $ sudo chown :www-data /var/www/html/.htaccess

            # 5. change permalinks
            # settings -> permalinks

1. config for wordpress

    1. wordpress.conf

            $ cd /etc/apache2/sites-available
            $ sudo pico wordpress.conf
            <VirtualHost *:8888>

                ServerName 127.0.0.1
                ServerAlias www.wp.com
                DocumentRoot /var/www/html/wordpress

                ErrorLog /var/log/apache2/wordpress.error.log
                CustomLog /var/log/apache2/wordpress.custom.log combined

                <Directory />
                Require all granted
                </Directory>

            </VirtualHost>

    1. ports.conf

            $ cd /etc/apache2
            $ sudo pico ports.conf
            # If you just change the port or add more ports here, you will likely also
            # have to change the VirtualHost statement in
            # /etc/apache2/sites-enabled/000-default.conf

            Listen 8080
            Listen 8888

            <IfModule ssl_module>
                    Listen 443
            </IfModule>

            <IfModule mod_gnutls.c>
                    Listen 443
            </IfModule>

            # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
            Listen 8800



### references

1. [how-to-install-wordpress-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-on-ubuntu-14-04)