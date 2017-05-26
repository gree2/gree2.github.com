---
layout: post
title: "setup ckan on centos"
description: ""
category: [setup, linux]
tags: [ckan, centos, postgresql, solr]
---
{% include JB/setup %}


### steps

1. install required packages

    1. update system

            $ yum install centos-release

            $ yum update
            $ shutdown -r now

    1. install some tools

            $ yum install wget policycoreutils-python

    1. install epel

            $ rpm -Uvh http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7.5.noarch.rpm

    1. install packages

            $ yum install xml-commons git subversion mercurial postgresql-server postgresql-devel \
            postgresql python-devel libxslt libxslt-devel libxml2 libxml2-devel python-virtualenv \
            gcc gcc-c++ make java-1.6.0-openjdk-devel java-1.6.0-openjdk tomcat tomcat-webapps \
            tomcat-admin-webapps xalan-j2 unzip policycoreutils-python mod_wsgi httpd

1. install ckan

    1. create user

            $ useradd -m -s /sbin/nologin -d /usr/lib/ckan -c "CKAN User" ckan

            # read access for httpd
            $ chmod 755 /usr/lib/ckan

    1.  switch to ckan user

            $ su -s /bin/bash - ckan

    1. install an isolated python env

            # called default to host ckan
            $ virtualenv --no-site-packages default
            $ default/bin/activate

    1. download and install ckan

            $ pip install --ignore-installed -e git+https://github.com/okfn/ckan.git@ckan-2.7.0#egg=ckan

    1. install python packages

            $ pip install --ignore-installed -r default/src/ckan/pip-requirements-docs.txt
            # xargs -a requirements.txt -n 1 pip install
            $ exit

1. configure postgresql

    1. start on boot

            $ systemctl enable postgresql.service

    1. initialize postgresql

            $ postgresql-setup initdb

    1. password login and `ident` login

            $ nano /var/lib/pgsql/data/pg_hba.conf
            local all postgres              ident
            local all all                   md5
            # ipv4 local connections
            host  all all      127.0.0.1/32 md5
            # ipv6 local connections
            host  all all      ::1/128      md5

    1. start postgresql

            $ systemctl start postgresql.service

    1. switch to postgres user

            $ su - postgres
            # list existing databases
            $ psql -l
            # check databases encoding UTF8

    1. create user and database for ckan

            $ createuser -S -D -R -P ckan_default
            $ createdb -O ckan_default ckan_default -E utf-8
            $ exit # exit postgres user env

1. create ckan configuration

    1. create dir for site's config files

            $ mkdir -p /etc/ckan/default
            $ chown -R ckan /etc/ckan/

    1. switch to `ckan` user

            $ su -s /bin/bash - ckan
            $ . default/bin/activate
            $ cd /usr/lib/ckan/default/src/ckan
            $ paster make-config ckan /etc/ckan/default/development.ini
            Distribution already installed:
              ckan 2.6.1 from /usr/lib/ckan/default/src/ckan
            Creating /etc/ckan/default/development.ini
            Now you should edit the config files
              /etc/ckan/default/development.ini

    1. edit `development.ini`

            $ nano /etc/ckan/default/development.ini
            ...
            sqlalchemy.url = postgresql://ckan_default:pass@localhost/ckan_default
            ckan.site_id = default
            solr_url = http://127.0.0.1:8091/solr/ckan-schema-2.3

1. setup apache solr

    1. download and install solr

            # ckan can not use the latest version of solr & requires 1.4.1
            $ curl http://archive.apache.org/dist/lucene/solr/1.4.1/apache-solr-1.4.1.tgz | tar xzf -

    1. create dirs to hold multiple solr cores

            $ mkdir -p \
            /usr/share/solr/core0 \
            /usr/share/solr/core1 \
            /var/lib/solr/data/core0 \
            /var/lib/solr/data/core1 \
            /etc/solr/core0 \
            /etc/solr/core1

    1. copy solr files

            $ cp apache-solr-1.4.1/dist/apache-solr-1.4.1.war /usr/share/solr
            # copy config file
            $ cp -r apache-solr-1.4.1/example/solr/conf /etc/solr/core0

            $ nano /etc/solr/core0/conf/solrconfig.xml
            # <dataDir>${solr.data.dir:./solr/data}</dataDir>
            <dataDir>${dataDir}</dataDir>
            $ cp -r /etc/solr/core0/conf /etc/solr/core1

    1. create symbolic link

            $ ln -s /etc/solr/core0/conf /usr/share/solr/core0/conf
            $ ln -s /etc/solr/core1/conf /usr/share/solr/core1/conf

    1. remove provided schema and link the schema files in ckan source

            $ rm -f /etc/solr/core0/conf/schema.xml
            $ ^0^1
            $ ln -s /usr/lib/ckan/default/src/ckan/ckan/config/solr/schema.xml /etc/solr/core0/conf/schema.xml
            $ ^0^1

    1. create `/etc/tomcat/Catalina/localhost/solr.xml`

            $ nano /etc/tomcat/Catalina/localhost/solr.xml
            <Context docBase="/usr/share/solr/apache-solr-1.4.1.war" debug="0" privileged="true" allowLinking="true" crossContext="true">
                <Environment name="solr/home" type="java.lang.String" value="/usr/share/solr" override="true" />
            </Context>

            $ nano /usr/share/solr/solr.xml
            <solr persistent="true" sharedLib="lib">
            <cores adminPath="/admin/cores">
                <core name="ckan-schema-2.3" instanceDir="core0"> <property name="dataDir" value="/var/lib/solr/data/core0" /></core>
                <core name="ckan-schema-1.4" instanceDir="core1"> <property name="dataDir" value="/var/lib/solr/data/core1" /></core>
            </cores>

    1. set permissions

            $ chown -R tomcat:tomcat /usr/share/solr /var/lib/solr

    1. enable tomcat

            $ systemctl enable tomcat.service
            Created symlink from /etc/systemd/system/multi-user.target.wants/tomcat.service to /usr/lib/systemd/system/tomcat.service.
            $ systemctl start tomcat.service

            $ firewall-cmd --zone=public --add-port=8091/tcp --permanent
            $ firewall-cmd --reload
            $ firewall-cmd --list-all
            $ open http://192.168.100.111:8091
            $ open http://192.168.100.111:8091/solr

1. create database tables

    1. switch to `ckan` user

            $ su -s /bin/bash - ckan
            $ . default/bin/activate
            $ cd default/src/ckan

    1. initialize ckan database

            $ paster db init -c /etc/ckan/default/development.ini
            Initialising DB: SUCCESS

1. setup datastore (optional)

1. link to who.ini

            # in virtualenv
            $ ln -s /usr/lib/ckan/default/src/ckan/who.ini /etc/ckan/default/who.ini

1. create a wsgi file

    1. /etc/ckan/default/apache.wsgi

            $ nano /etc/ckan/default/apache.wsgi
            import os
            activate_this = os.path.join('/usr/lib/ckan/default/bin/activate_this.py')
            execfile(activate_this, dict(__file__=activate_this))

            from paste.deploy import loadapp
            config_filepath = os.path.join(os.path.dirname(os.path.abspath(__file__)), 'development.ini')
            from paste.script.util.logging_config import fileConfig
            fileConfig(config_filepath)
            application = loadapp('config:%s' % config_filepath)
            $ exit # exit ckan user

1. create apache config file

    1. /etc/httpd/conf.d/ckan_default.conf

            $ nano /etc/httpd/conf.d/ckan_default.conf

            WSGISocketPrefix /var/run/wsgi
            <VirtualHost 0.0.0.0:8092>
                ServerName 192.168.100.111
                ServerAlias 192.168.100.111
                WSGIScriptAlias / /etc/ckan/default/apache.wsgi

                # Pass authorization info on (needed for rest api).
                WSGIPassAuthorization On

                # Deploy as a daemon (avoids conflicts between CKAN instances).
                WSGIDaemonProcess ckan_default display-name=ckan_default processes=2 threads=15

                WSGIProcessGroup ckan_default

                # Add this to avoid Apache show error:
                # "AH01630: client denied by server configuration: /etc/ckan/default/apache.wsgi"
                <Directory /etc/ckan/default>
                    Options All
                    AllowOverride All
                    Require all granted
                </Directory>

                ErrorLog /var/log/httpd/ckan_default.error.log
                CustomLog /var/log/httpd/ckan_default.custom.log combined
            </VirtualHost>

    1. open port 8092

            $ firewall-cmd --zone=public --add-port=8092/tcp --permanent
            $ firewall-cmd --reload
            $ firewall-cmd --list-all

1. configure apache

    1. start on boot

            $ chkconfig httpd on

    1. start httpd

            $ systemctl start httpd

1. configure firewalld

            $ firewall-cmd --zone=public --add-port=8092/tcp --permanet

1. connect to ckan

            $ open http://192.168.100.111:8092

### fixed

1. setuptools version

    1. error

            pkg_resources.VersionConflict: (setuptools 0.9.8 (/usr/lib/ckan/default/lib/python2.7/site-packages), Requirement.parse('setuptools>=17.1'))

    1. [https://stackoverflow.com/questions/1956646/determining-version-of-easy-install-setuptools](https://stackoverflow.com/questions/1956646/determining-version-of-easy-install-setuptools)

            $ easy_install --version
            setuptools 35.0.2 from /usr/lib/ckan/default/lib/python2.7/site-packages (Python 2.7)

    1. fix by update setuptools

            $ pip install -U setuptools

1. mod_wsgi.so

    1. [lack-of-python-shared-library](http://modwsgi.readthedocs.io/en/develop/user-guides/installation-issues.html#lack-of-python-shared-library)

            $ ldd mod_wsgi.so
            linux-vdso.so.1 =>  (0x00007fff78bd1000)
            libpython2.7.so.1.0 => /lib64/libpython2.7.so.1.0 (0x00007fad3f617000)
            libpthread.so.0 => /lib64/libpthread.so.0 (0x00007fad3f3fb000)
            libdl.so.2 => /lib64/libdl.so.2 (0x00007fad3f1f6000)
            libutil.so.1 => /lib64/libutil.so.1 (0x00007fad3eff3000)
            libm.so.6 => /lib64/libm.so.6 (0x00007fad3ecf1000)
            libc.so.6 => /lib64/libc.so.6 (0x00007fad3e92f000)
            /lib64/ld-linux-x86-64.so.2 (0x00007fad3fc21000)

### references

1. [https://stackoverflow.com/questions/22250483/stop-pip-from-failing-on-single-package-when-installing-with-requirements-txt](https://stackoverflow.com/questions/22250483/stop-pip-from-failing-on-single-package-when-installing-with-requirements-txt)

1. apache

    1. [https://stackoverflow.com/questions/289586/version-of-apache-installed-on-a-debian-machine](https://stackoverflow.com/questions/289586/version-of-apache-installed-on-a-debian-machine)

            $ apachectl -V
            Server version: Apache/2.4.6 (CentOS)
            Server built:   Apr 12 2017 21:03:28
            Server's Module Magic Number: 20120211:24
            Server loaded:  APR 1.4.8, APR-UTIL 1.5.2
            Compiled using: APR 1.4.8, APR-UTIL 1.5.2
            Architecture:   64-bit
            Server MPM:     prefork
              threaded:     no
                forked:     yes (variable process count)
            Server compiled with....
             -D APR_HAS_SENDFILE
             -D APR_HAS_MMAP
             -D APR_HAVE_IPV6 (IPv4-mapped addresses enabled)
             -D APR_USE_SYSVSEM_SERIALIZE
             -D APR_USE_PTHREAD_SERIALIZE
             -D SINGLE_LISTEN_UNSERIALIZED_ACCEPT
             -D APR_HAS_OTHER_CHILD
             -D AP_HAVE_RELIABLE_PIPED_LOGS
             -D DYNAMIC_MODULE_LIMIT=256
             -D HTTPD_ROOT="/etc/httpd"
             -D SUEXEC_BIN="/usr/sbin/suexec"
             -D DEFAULT_PIDLOG="/run/httpd/httpd.pid"
             -D DEFAULT_SCOREBOARD="logs/apache_runtime_status"
             -D DEFAULT_ERRORLOG="logs/error_log"
             -D AP_TYPES_CONFIG_FILE="conf/mime.types"
             -D SERVER_CONFIG_FILE="conf/httpd.conf"

    1. [compiled in modules](http://modwsgi.readthedocs.io/en/develop/user-guides/checking-your-installation.html#apache-modules-loaded)

            $ httpd -l
            Compiled in modules:
              core.c
              mod_so.c
              http_core.c

    1. [loaded modules](http://modwsgi.readthedocs.io/en/develop/user-guides/checking-your-installation.html#apache-modules-loaded)

            $ httpd -M
            Loaded Modules:
             core_module (static)
             so_module (static)
             http_module (static)
             access_compat_module (shared)
             ...
             php5_module (shared)
             wsgi_module (shared)
