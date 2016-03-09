---
layout: post
title: "install nagiosgraph"
description: ""
category: [ubuntu]
tags: [nagios, nagiosgraph]
---
{% include JB/setup %}


### [nagios core 4 installation on ubuntu 12.04](https://raymii.org/s/tutorials/Nagios_Core_4_Installation_on_Ubuntu_12.04.html)

1. install

    1. download the latest version

            $ cd Downloads
            wget http://iweb.dl.sourceforge.net/project/nagiosgraph/nagiosgraph/1.5.2/nagiosgraph-1.5.2.tar.gz

    1. extract it

            $ tar -zxf nagiosgraph-1.5.2.tar.gz

    1. check that we have all the dependencies installed

            $ cd nagiosgraph-1.5.2
            $ ./install.pl --check-prereq
            checking required PERL modules
              Carp...1.3301
              CGI...4.09
              Data::Dumper...2.151_01
              Digest::MD5...2.53
              File::Basename...2.85
              File::Find...1.27
              MIME::Base64...3.14
              POSIX...1.38_03
              RRDs... ***FAIL***
              Time::HiRes...1.9726
            checking optional PERL modules
              GD... ***FAIL***
              Nagios::Config... ***FAIL***
            checking nagios installation
              found nagios exectuable at /usr/local/nagios/bin/nagios
              found nagios init script at /etc/init.d/nagios
            checking web server installation
              found apache executable at /usr/sbin/apache2
              found apache init script at /etc/init.d/apache2
            
            *** one or more problems were detected!
            # ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            # check fixed

    1. start the installation

            $ ./install.pl --layout standalone --prefix /usr/local/nagiosgraph
            # give the default answer to all questions except the below one
            # and input apache config directory

                username or userid of Nagios user? [nagios] 
                username or userid of web server user? [www-data] 
            ==> Modify the Nagios configuration? [n] y
                Path of Nagios configuration file? [/usr/local/nagios/etc/nagios.cfg] 
                Path of Nagios commands file? [/usr/local/nagios/etc/objects/commands.cfg] 
            ==> Modify the Apache configuration? [n] y
            ==> Path of Apache configuration directory? /etc/apache2
            # ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            # check fixed

    1. on ubuntu 14.04 change apache configuration

            $ sudo pico /usr/local/nagiosgraph/etc/nagiosgraph-apache.conf
            # change it to
            # enable nagiosgraph CGI scripts
            ScriptAlias /nagiosgraph/cgi-bin "/usr/local/nagiosgraph/cgi"
            <Directory "/usr/local/nagiosgraph/cgi">
               Options ExecCGI
               AllowOverride None
               Require all granted
            </Directory>
            # enable nagiosgraph CSS and JavaScript
            Alias /nagiosgraph "/usr/local/nagiosgraph/share"
            <Directory "/usr/local/nagiosgraph/share">
               Options None
               AllowOverride None
               Require all granted
            </Directory>

    1. restart nagios and apache

            $ sudo service nagios restart
            $ sudo service apache2 restart

    1. view graphs at [https://node5/nagiosgraph/cgi-bin/show.cgi](https://node5/nagiosgraph/cgi-bin/show.cgi)

1. integrate these graphs into nagios

    1. place a js url to the graphs inside a service check

            action_url      /nagiosgraph/cgi-bin/show.cgi?host=$HOSTNAME$&service=$SERVICEDESC$&geom=1000x200' onMouseOver='showGraphPopup(this)' onMouseOut='hideGraphPopup()' rel='/nagiosgraph/cgi-bin/showgraph.cgi?host=$HOSTNAME$&service=$SERVICEDESC$

    1. for example the load of the system

            define service {
                    use                     local-service
                    host_name               localhost
                    service_description     Load
                    check_command           check_local_load!5.0,4.0,3.0!10.0,6.0,4.0
                    action_url              /nagiosgraph/cgi-bin/show.cgi?host=$HOSTNAME$&service=$SERVICEDESC$&geom=1000x200' onMouseOver='showGraphPopup(this)' onMouseOut='hideGraphPopup()' rel='/nagiosgraph/cgi-bin/showgraph.cgi?host=$HOSTNAME$&service=$SERVICEDESC$
            }

    1. for ping check you can show both `rta` and `packet loss`

            define service {
                    use                     local-service
                    host_name               localhost
                    service_description     PING
                    check_command           check_ping!100.0,20%!500.0,60%
                    action_url              /nagiosgraph/cgi-bin/show.cgi?host=$HOSTNAME$&service=$SERVICEDESC$&db=pl,data&db=pl,warn&db=pl,crit&geom=1000x200' onMouseOver='showGraphPopup(this)' onMouseOut='hideGraphPopup()' rel='/nagiosgraph/cgi-bin/showgraph.cgi?host=$HOSTNAME$&service=$SERVICEDESC$&db=pl,data&db=pl,warn&db=pl,crit
                    notes_url               /nagiosgraph/cgi-bin/show.cgi?host=$HOSTNAME$&service=$SERVICEDESC$&db=rta,data&db=rta,warn&db=rta,crit&geom=1000x200' onMouseOver='showGraphPopup(this)' onMouseOut='hideGraphPopup()' rel='/nagiosgraph/cgi-bin/showgraph.cgi?host=$HOSTNAME$&service=$SERVICEDESC$&db=rta,data&db=rta,warn&db=rta,crit
            }

    1. include nagios graph js in nagios to make sure the mouseover works

            $ sudo pico /usr/local/nagios/share/ssi/common-header.ssi
            # place the following in
            <script type="text/javascript" src="/nagiosgraph/nagiosgraph.js"></script>

    1. restart nagios

            $ sudo service nagios restart

### [steps to configure nagiosgraph with nagios core](http://www.linuxfunda.com/2013/04/02/steps-to-configure-nagiosgraph-with-nagios-core/)

1. config

    1. change to nagios core configuration directory

            $ cd /usr/local/nagios/etc

    1. add following lines to the end of `nagios.cfg`

            $ sudo pico nagios.cfg
            # process nagios performance data using nagiosgraph
            process_performance_data=1
            service_perfdata_file=/tmp/perfdata.log
            service_perfdata_file_template=$LASTSERVICECHECK$||$HOSTNAME$||$SERVICEDESC$||$SERVICEOUTPUT$||$SERVICEPERFDATA$
            service_perfdata_file_mode=a
            service_perfdata_file_processing_interval=30
            service_perfdata_file_processing_command=process-service-perfdata-for-nagiosgraph

    1. add following command definition to `commands.cfg`

            $ sudo pico commands.cfg
            # command to process nagios performance data for nagiosgraph
            define command {
                command_name process-service-perfdata-for-nagiosgraph
                command_line /usr/local/nagiosgraph/bin/insert.pl
            }

    1. add following line at the end of `httpd.conf`

            # in a local install of apache httpd
            # this `httpd.conf` is normarlly in
            # /usr/local/apache/conf/httpd.conf

            # on debian-derived systems
            # it may be /etc/apache2/apache2.conf

            $ sudo pico /etc/apache2/apache2.conf
            Include /usr/local/nagiosgraph/etc/nagiosgraph-apache.conf

    1. [validate config of both apache httpd server](http://makandracards.com/makandra/1263-check-apache-config-files-syntax) and nagios core server and restart them both

            $ sudo apache2ctl configtest
            or
            $ sudo apache2ctl -t
            $ sudo service apache2 restart

            $ sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
            $ sudo service nagios restart

    1. define an action url for the services

            # 1. edit the template.cfg
            # append a new service template
            define service {
                name nagiosgraph
                action_url /nagiosgraph/cgi-bin/show.cgi?host=$HOSTNAME$&$SERVICEDESC$
                register 0
            }

            # 2. inherit form `nagiosgraph` service
            define service {
                use                 genric-service, nagiosgraph
                host_name           localhost
                service_description PING
                check_command       check_ping!100,10%!200,20%
            }

    1. validate config and restart nagios core server

            $ /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
            $ sudo service nagios restart

### fixed

1. [fixed check-prereq](http://www.linuxfunda.com/2013/04/02/steps-to-configure-nagiosgraph-with-nagios-core/)

    1. install `rrds` and `gd` modules

            # these are all reasonably standard perl libraries
            $ apt-get install librrds-perl libgd-gd2-perl

    1. [install](https://support.nagios.com/forum/viewtopic.php?f=7&t=28276) `Nagios::Config`

            $ perl -MCPAN -e shell
            cpan[1]> install Nagios::Config
            cpan[2]> exit

1. `./install.pl` full output

            $ ./install.pl --layout standalone --prefix /usr/local/nagiosgraph
            checking required PERL modules
              Carp...1.3301
              CGI...4.09
              Data::Dumper...2.151_01
              Digest::MD5...2.53
              File::Basename...2.85
              File::Find...1.27
              MIME::Base64...3.14
              POSIX...1.38_03
              RRDs...1.4008
              Time::HiRes...1.9726
            checking optional PERL modules
              GD...2.46
              Nagios::Config...36
            checking nagios installation
              found nagios exectuable at /usr/local/nagios/bin/nagios
              found nagios init script at /etc/init.d/nagios
            checking web server installation
              found apache executable at /usr/sbin/apache2
              found apache init script at /etc/init.d/apache2
            Destination directory (prefix)? [/usr/local/nagiosgraph] 
            Location of configuration files (etc-dir)? [/usr/local/nagiosgraph/etc] 
            Location of executables? [/usr/local/nagiosgraph/bin] 
            Location of CGI scripts? [/usr/local/nagiosgraph/cgi] 
            Location of documentation (doc-dir)? [/usr/local/nagiosgraph/doc] 
            Location of examples? [/usr/local/nagiosgraph/examples] 
            Location of CSS and JavaScript files? [/usr/local/nagiosgraph/share] 
            Location of utilities? [/usr/local/nagiosgraph/util] 
            Location of state files (var-dir)? [/usr/local/nagiosgraph/var] 
            Location of RRD files? [/usr/local/nagiosgraph/var/rrd] 
            Location of log files (log-dir)? [/usr/local/nagiosgraph/var/log] 
            Path of log file? [/usr/local/nagiosgraph/var/log/nagiosgraph.log] 
            Path of CGI log file? [/usr/local/nagiosgraph/var/log/nagiosgraph-cgi.log] 
            Base URL? [/nagiosgraph] 
            URL of CGI scripts? [/nagiosgraph/cgi-bin] 
            URL of CSS file? [/nagiosgraph/nagiosgraph.css] 
            URL of JavaScript file? [/nagiosgraph/nagiosgraph.js] 
            URL of Nagios CGI scripts? [/nagios/cgi-bin] 
            Path of Nagios performance data file? [/tmp/perfdata.log] 
            username or userid of Nagios user? [nagios] 
            username or userid of web server user? [www-data] 
            Modify the Nagios configuration? [n] y
            Path of Nagios configuration file? [/usr/local/nagios/etc/nagios.cfg] 
            Path of Nagios commands file? [/usr/local/nagios/etc/objects/commands.cfg] 
            Modify the Apache configuration? [n] y
            Path of Apache configuration directory? /etc/apache2
            configuration:
              ng_prefix            /usr/local/nagiosgraph
              ng_etc_dir           /usr/local/nagiosgraph/etc
              ng_bin_dir           /usr/local/nagiosgraph/bin
              ng_cgi_dir           /usr/local/nagiosgraph/cgi
              ng_doc_dir           /usr/local/nagiosgraph/doc
              ng_examples_dir      /usr/local/nagiosgraph/examples
              ng_www_dir           /usr/local/nagiosgraph/share
              ng_util_dir          /usr/local/nagiosgraph/util
              ng_var_dir           /usr/local/nagiosgraph/var
              ng_rrd_dir           /usr/local/nagiosgraph/var/rrd
              ng_log_dir           /usr/local/nagiosgraph/var/log
              ng_log_file          /usr/local/nagiosgraph/var/log/nagiosgraph.log
              ng_cgilog_file       /usr/local/nagiosgraph/var/log/nagiosgraph-cgi.log
              ng_url               /nagiosgraph
              ng_cgi_url           /nagiosgraph/cgi-bin
              ng_css_url           /nagiosgraph/nagiosgraph.css
              ng_js_url            /nagiosgraph/nagiosgraph.js
              nagios_cgi_url       /nagios/cgi-bin
              nagios_perfdata_file /tmp/perfdata.log
              nagios_user          nagios
              www_user             www-data
              modify_nagios_config y
              nagios_config_file   /usr/local/nagios/etc/nagios.cfg
              nagios_commands_file /usr/local/nagios/etc/objects/commands.cfg
              modify_apache_config y
              apache_config_dir    /etc/apache2
              apache_config_file   
            Continue with this configuration? [y] y
            mkdir /usr/local/nagiosgraph
            mkdir /usr/local/nagiosgraph/etc
            copy etc/nagiosgraph_es.conf to /usr/local/nagiosgraph/etc/nagiosgraph_es.conf
            copy etc/nagiosgraph_fr.conf to /usr/local/nagiosgraph/etc/nagiosgraph_fr.conf
            copy etc/groupdb.conf to /usr/local/nagiosgraph/etc/groupdb.conf
            copy etc/nagiosgraph.conf to /usr/local/nagiosgraph/etc/nagiosgraph.conf
            copy etc/rrdopts.conf to /usr/local/nagiosgraph/etc/rrdopts.conf
            copy etc/servdb.conf to /usr/local/nagiosgraph/etc/servdb.conf
            copy etc/datasetdb.conf to /usr/local/nagiosgraph/etc/datasetdb.conf
            copy etc/hostdb.conf to /usr/local/nagiosgraph/etc/hostdb.conf
            copy etc/nagiosgraph_de.conf to /usr/local/nagiosgraph/etc/nagiosgraph_de.conf
            copy etc/labels.conf to /usr/local/nagiosgraph/etc/labels.conf
            copy etc/access.conf to /usr/local/nagiosgraph/etc/access.conf
            copy etc/map to /usr/local/nagiosgraph/etc/map
            copy etc/ngshared.pm to /usr/local/nagiosgraph/etc
            replace text in /usr/local/nagiosgraph/etc/nagiosgraph.conf
            move /usr/local/nagiosgraph/etc/nagiosgraph.conf-bak to /usr/local/nagiosgraph/etc/nagiosgraph.conf
            replace text in /usr/local/nagiosgraph/etc/nagiosgraph.conf
            move /usr/local/nagiosgraph/etc/nagiosgraph.conf-bak to /usr/local/nagiosgraph/etc/nagiosgraph.conf
            write stub to /usr/local/nagiosgraph/etc/nagiosgraph-nagios.cfg
            write stub to /usr/local/nagiosgraph/etc/nagiosgraph-commands.cfg
            write stub to /usr/local/nagiosgraph/etc/nagiosgraph-apache.conf
            mkdir /usr/local/nagiosgraph/cgi
            copy cgi/showhost.cgi to /usr/local/nagiosgraph/cgi
            replace text in /usr/local/nagiosgraph/cgi/showhost.cgi
            move /usr/local/nagiosgraph/cgi/showhost.cgi-bak to /usr/local/nagiosgraph/cgi/showhost.cgi
            chmod 755 on /usr/local/nagiosgraph/cgi/showhost.cgi
            copy cgi/show.cgi to /usr/local/nagiosgraph/cgi
            replace text in /usr/local/nagiosgraph/cgi/show.cgi
            move /usr/local/nagiosgraph/cgi/show.cgi-bak to /usr/local/nagiosgraph/cgi/show.cgi
            chmod 755 on /usr/local/nagiosgraph/cgi/show.cgi
            copy cgi/showgraph.cgi to /usr/local/nagiosgraph/cgi
            replace text in /usr/local/nagiosgraph/cgi/showgraph.cgi
            move /usr/local/nagiosgraph/cgi/showgraph.cgi-bak to /usr/local/nagiosgraph/cgi/showgraph.cgi
            chmod 755 on /usr/local/nagiosgraph/cgi/showgraph.cgi
            copy cgi/export.cgi to /usr/local/nagiosgraph/cgi
            replace text in /usr/local/nagiosgraph/cgi/export.cgi
            move /usr/local/nagiosgraph/cgi/export.cgi-bak to /usr/local/nagiosgraph/cgi/export.cgi
            chmod 755 on /usr/local/nagiosgraph/cgi/export.cgi
            copy cgi/showconfig.cgi to /usr/local/nagiosgraph/cgi
            replace text in /usr/local/nagiosgraph/cgi/showconfig.cgi
            move /usr/local/nagiosgraph/cgi/showconfig.cgi-bak to /usr/local/nagiosgraph/cgi/showconfig.cgi
            chmod 755 on /usr/local/nagiosgraph/cgi/showconfig.cgi
            copy cgi/showgroup.cgi to /usr/local/nagiosgraph/cgi
            replace text in /usr/local/nagiosgraph/cgi/showgroup.cgi
            move /usr/local/nagiosgraph/cgi/showgroup.cgi-bak to /usr/local/nagiosgraph/cgi/showgroup.cgi
            chmod 755 on /usr/local/nagiosgraph/cgi/showgroup.cgi
            copy cgi/showservice.cgi to /usr/local/nagiosgraph/cgi
            replace text in /usr/local/nagiosgraph/cgi/showservice.cgi
            move /usr/local/nagiosgraph/cgi/showservice.cgi-bak to /usr/local/nagiosgraph/cgi/showservice.cgi
            chmod 755 on /usr/local/nagiosgraph/cgi/showservice.cgi
            copy cgi/testcolor.cgi to /usr/local/nagiosgraph/cgi
            replace text in /usr/local/nagiosgraph/cgi/testcolor.cgi
            move /usr/local/nagiosgraph/cgi/testcolor.cgi-bak to /usr/local/nagiosgraph/cgi/testcolor.cgi
            chmod 755 on /usr/local/nagiosgraph/cgi/testcolor.cgi
            mkdir /usr/local/nagiosgraph/bin
            copy lib/insert.pl to /usr/local/nagiosgraph/bin
            replace text in /usr/local/nagiosgraph/bin/insert.pl
            move /usr/local/nagiosgraph/bin/insert.pl-bak to /usr/local/nagiosgraph/bin/insert.pl
            chmod 755 on /usr/local/nagiosgraph/bin/insert.pl
            mkdir /usr/local/nagiosgraph/share
            copy share/nagiosgraph.css to /usr/local/nagiosgraph/share/nagiosgraph.css
            copy share/nagiosgraph.js to /usr/local/nagiosgraph/share
            mkdir /usr/local/nagiosgraph/doc
            copy AUTHORS to /usr/local/nagiosgraph/doc
            copy CHANGELOG to /usr/local/nagiosgraph/doc
            copy INSTALL to /usr/local/nagiosgraph/doc
            copy README to /usr/local/nagiosgraph/doc
            copy TODO to /usr/local/nagiosgraph/doc
            mkdir /usr/local/nagiosgraph/examples
            copy examples/nagiosgraph-nagios.cfg to /usr/local/nagiosgraph/examples
            copy examples/graphed-host.cfg to /usr/local/nagiosgraph/examples
            copy examples/nagiosgraph-apache.conf to /usr/local/nagiosgraph/examples
            copy examples/map_examples to /usr/local/nagiosgraph/examples
            copy examples/map_mwall to /usr/local/nagiosgraph/examples
            copy examples/map_1_3 to /usr/local/nagiosgraph/examples
            copy examples/nagiosgraph.2.css to /usr/local/nagiosgraph/examples
            copy examples/map_minimal to /usr/local/nagiosgraph/examples
            copy examples/nagiosgraph-commands.cfg to /usr/local/nagiosgraph/examples
            copy examples/map_1_4_3 to /usr/local/nagiosgraph/examples
            copy examples/map_1_4_4 to /usr/local/nagiosgraph/examples
            copy examples/nagiosgraph-logrotate to /usr/local/nagiosgraph/examples
            copy examples/graphed-service.cfg to /usr/local/nagiosgraph/examples
            copy examples/map_1_4_5 to /usr/local/nagiosgraph/examples
            copy examples/insert.sh to /usr/local/nagiosgraph/examples
            copy examples/nagiosgraph.1.css to /usr/local/nagiosgraph/examples
            copy share/graph.gif to /usr/local/nagiosgraph/examples
            copy share/nagiosgraph.ssi to /usr/local/nagiosgraph/examples
            mkdir /usr/local/nagiosgraph/util
            copy utils/testentry.pl to /usr/local/nagiosgraph/util
            copy utils/flat2hier.pl to /usr/local/nagiosgraph/util
            chmod 755 on /usr/local/nagiosgraph/util/testentry.pl
            chmod 755 on /usr/local/nagiosgraph/util/flat2hier.pl
            mkdir /usr/local/nagiosgraph/var/rrd
            chmod 755 on /usr/local/nagiosgraph/var/rrd
            chown nagios,- on /usr/local/nagiosgraph/var/rrd
            mkdir /usr/local/nagiosgraph/var/log
            chmod 755 on /usr/local/nagiosgraph/var/log
            touching /usr/local/nagiosgraph/var/log/nagiosgraph.log
            chmod 644 on /usr/local/nagiosgraph/var/log/nagiosgraph.log
            chown nagios,- on /usr/local/nagiosgraph/var/log/nagiosgraph.log
            touching /usr/local/nagiosgraph/var/log/nagiosgraph-cgi.log
            chmod 644 on /usr/local/nagiosgraph/var/log/nagiosgraph-cgi.log
            chown www-data,- on /usr/local/nagiosgraph/var/log/nagiosgraph-cgi.log
            append to /usr/local/nagios/etc/nagios.cfg
            copy /usr/local/nagios/etc/nagios.cfg to /usr/local/nagios/etc/nagios.cfg.tmp
            move /usr/local/nagios/etc/nagios.cfg to /usr/local/nagios/etc/nagios.cfg-20150720.1652
            move /usr/local/nagios/etc/nagios.cfg.tmp to /usr/local/nagios/etc/nagios.cfg
            append to /usr/local/nagios/etc/objects/commands.cfg
            copy /usr/local/nagios/etc/objects/commands.cfg to /usr/local/nagios/etc/objects/commands.cfg.tmp
            move /usr/local/nagios/etc/objects/commands.cfg to /usr/local/nagios/etc/objects/commands.cfg-20150720.1652
            move /usr/local/nagios/etc/objects/commands.cfg.tmp to /usr/local/nagios/etc/objects/commands.cfg
            move /usr/local/nagiosgraph/etc/nagiosgraph-apache.conf to /etc/apache2/nagiosgraph.conf
            
              * Check the nagios configuration:
            
            /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
            
              * Restart nagios to start data collection:
            
            /etc/init.d/nagios restart
            
              * Restart apache to enable display of graphs:
            
            /etc/init.d/apache2 restart
            
              * To enable graph links and mouseovers, see README sections:
                   Displaying Per-Service and Per-Host Graph Icons and Links
                   Displaying Graphs in Nagios Mouseovers
