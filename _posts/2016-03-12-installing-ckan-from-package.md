---
layout: post
title: "installing ckan from package"
description: ""
category: [setup]
tags: [ckan, ubuntu, postgresql, solr]
---
{% include JB/setup %}


### stpes

1. install the ckan package

    1. update ubuntu's package index

            $ sudo apt-get update

    1. install packages ckan requires

            $ sudo apt-get install -y nginx apache2 libapache2-mod-wsgi libpq5

            $ sudo a2enmod wsgi
            $ sudo service apache2 restart

    1. download ckan package

            $ wget http://packaging.ckan.org/python-ckan_2.5-trusty_amd64.deb

    1. install the ckan package

            $ sudo dpkg -i python-ckan_2.5-trusty_amd64.deb

1. install postgresql and solr

    1. install

            $ sudo apt-get install -y postgresql solr-jetty

    1. [setup solr for ckan]({% post_url 2016-03-12-setup-solr-for-ckan %})

    1. [setup postgresql for ckan]({% post_url 2016-03-12-setup-postgresql-for-ckan %})

1. update config and init database

    1. edit ckan config file

            $ pico /etc/ckan/default/production.ini

            # setup site_id
            ckan.site_id = default

            # setup site_url
            ckan.site_url = http://demo.ckan.org

    1. init ckan database

            $ sudo ckan db init

    1. optionally setup datastore and datapusher check [datastore extension](http://docs.ckan.org/en/latest/maintaining/datastore.html)

    1. optionally enable file uploads check [filestore and file uploads](http://docs.ckan.org/en/latest/maintaining/filestore.html)

1. restart apache and nginx

            $ sudo service apache2 restart
            $ sudo service nginx restart

1. you're done

            # in web browser
            http://localhost

### fixed

1. [Server Error An internal server error occurred](https://github.com/ckan/ckanext-pages/issues/23)

    1. install packages

            $ apt-get install libxml2-dev libxslt1-dev python-dev
            $ apt-get install python-lxml