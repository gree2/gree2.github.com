---
layout: post
title: "install mrtg on ubuntu"
description: ""
category: [ubuntu]
tags: [mrtg, snmp, indexmaker, cfgmaker, cron]
---
{% include JB/setup %}


### solutions

1. [install mrtg ubuntu 11.10 debian squeeze](http://colekcolek.com/2012/02/23/install-mrtg-ubuntu-11-10-debian-squeeze/)

    1. install `mrtg` and `snmp`

            $ sudo apt-get install snmpd mrtg

    1. configure `snmp`

            $ sudo mv /etc/snmp/snmpd.conf /etc/snmp/snmpd.conf.bak
            $ sudo pico /etc/snmp/snmpd.conf
            rocommunity  public
            syslocation  "hadoop"
            syscontact  hqlgree2@gmail.com
            com2sec public localhost public
            group public v1 public
            group public v2c public
            group public usm public
            view all included .1
            access public "" any noauth exact all none none

    1. check `/etc/default/snmpd`

            $ sudo pico /etc/default/snmpd
            ...
            SNMPDRUN=yes
            SNMPDOPTS='-Lsd -Lf /dev/null -u snmp -g snmp -I -smux -p /var/run/snmpd.pid -c /etc/snmp/snmpd.conf'
            TRAPDRUN=yes
            SNMPDCOMPAT=yes

    1. restart `snmpd`

            $ sudo /etc/init.d/snmpd restart

    1. configure mrtg

            # 1. create folder
            $ sudo mkdir /var/www/mrtg
            $ sudo chmod o+w /var/www/mrtg

            # 2. create configuration file
            $ sudo cfgmaker --global 'WorkDir:/var/www/mrtg' \
            --ifref=name --ifdesc=eth --global 'Options[_]: bits' \
            --output /etc/mrtg.cfg public@localhost

            # 3. create index file
            $ sudo indexmaker /etc/mrtg.cfg > /var/www/mrtg/index.html

            # 4. run mrtg
            $ sudo env LANG=C /usr/bin/mrtg /etc/mrtg.cfg

    1. create scheduled job

            $ sudo crontab -e
            */5 * * * * env LANG=C /usr/bin/mrtg /etc/mrtg.cfg

1. [nagios switch-check_mrtgtraf: unable to open mrtg log file
](https://support.nagios.com/forum/viewtopic.php?f=7&t=32639#p137442)

    1. place output files in `/etc/mrtg/conf.d`

            # change it from
            $ cfgmaker --global 'WorkDir:/var/www/html/mrtg' \
            --global 'Options[_]: bits,growright' \
            --output /etc/mrtg/mrtg.cfg public@192.168.1.253

            # to
            $ cfgmaker --global 'WorkDir:/var/www/html/mrtg' \
            --global 'Options[_]: bits,growright' \
            --output /etc/mrtg/conf.d/192.168.1.253.cfg \
            public@192.168.1.253

            # and then on your next one you add
            $ cfgmaker --global 'WorkDir:/var/www/html/mrtg' \
            --global 'Options[_]: bits,growright' \
            --output /etc/mrtg/conf.d/192.168.2.253.cfg \
            public@192.168.2.253

1. [how to install and configure mrtg on ubuntu server](https://www.iceflatline.com/2009/08/how-to-install-and-configure-mrtg-on-ubuntu-server/)

    1. install `mrtg`

            $ sudo apt-get update
            $ sudo apt-get install mrtg
            # yes => file permissions 640
            # no  => file permissions 644
                        
    1. sample configure file

            $ sudo cat /etc/mrtg.cfg
            # full listing of what
            # mrtg installs and where
            $ sudo updatedb && locate mrtg

    1. keep things tidy

            # mv mrtg.cfg into a folder
            $ sudo mkdir /etc/mrtg
            $ sudo mv /etc/mrtg.cfg /etc/mrtg

    1. configure

            # note 192.168.1.253 is your router's ip
            $ sudo cfgmaker --output=/etc/mrtg/mrtg.cfg public@192.168.1.253

            # two or more router
            $ sudo cfgmaker --output=/etc/mrtg/mrtg.cfg public@192.168.1.253 public@192.168.2.253

            # uncomment
            $ sudo pico /etc/mrtg/mrtg.cfg
            ...
            # under debian
            WorkDir: /var/www/mrtg
            ...
            # under global defaults
            Options[_]: growright, bits
            ...
            # under EnableIPv6: no
            # of global defaults
            RunAsDaemon: Yes
            Interval: 5

            # this means that every 5 minutes
            # mrtg will poll the snmp service
            # in your gateway/router and update its graphs

            # by default mrtg graphs grow to the left
            # so by adding `growright` will flips the graphs

            # `bits` means that the monitored traffice values
            # obtained from gateway/router are multiplied by 8
            # displayed bits/s instead of bytes/s

    1. create web page

            # 1. create page
            $ sudo mkdir /var/www/mrtg
            $ sudo indexmaker --output=/var/www/mrtg/index.html /etc/mrtg/mrtg.cfg

            # 2. config apache2
            $ sudo pico /etc/apache2/apache.conf
            ...
            Alias /mrtg "/var/www/mrtg/"
            <Directory "/var/www/mrtg">
                Options None
                AllowOverride None
                Require all granted
            </Directory>

            # 3. restart apache
            $ sudo service apache2 restart

            # 4. fix `...reliably determine the server's fully qualified domain name...`
            # add following line to
            $ sudo pico /etc/apache2/apache.conf
            ...
            ServerName localhost:80

    1. start

            $ sudo env LANG=C /usr/bin/mrtg /etc/mrtg/mrtg.cfg --logging /var/log/mrtg.log

    1. operate

            # automate the process of running mrtg
            $ cd
            $ pico mrtg
            #! /bin/sh
            ### BEGIN INIT INFO
            # Provides:          mrtg
            # Required-Start:    
            # Required-Stop:     
            # Default-Start:     2 3 4 5
            # Default-Stop:      0 1 6
            # Short-Description: mrtg init script
            # Description:       This file is used to start, stop, restart, 
            #                    and determined status of the mrtg daemon.
            # Author:            iceflatline <iceflatline@gmail.com>
            ### END INIT INFO

            ### START OF SCRIPT
            set -e
            # PATH should only include /usr/* if it runs after the mountnfs.sh script
            PATH=/sbin:/usr/sbin:/bin:/usr/bin
            DESC="mrtg"
            NAME=mrtg
            DAEMON=/usr/bin/$NAME
            DAEMON_ARGS="/etc/mrtg/mrtg.cfg --logging /var/log/mrtg.log"
            PIDFILE=/etc/mrtg/$NAME.pid
            SCRIPTNAME=/etc/init.d/$NAME

            # Exit if the mrtg package is not installed
            [ -x "$DAEMON" ] || exit 0

            # Load the VERBOSE setting and other rcS variables
            . /lib/init/vars.sh

            # Define LSB log_* functions.
            # Depend on lsb-base (>= 3.0-6) to ensure that this file is present.
            . /lib/lsb/init-functions

            # Function that starts the mrtg daemon
            start()
            {
                env LANG=C start-stop-daemon --start --quiet \
                --exec $DAEMON -- $DAEMON_ARGS
            }

            # Function that stops the mrtg daemon
            stop()
            {
                start-stop-daemon --stop --quiet --retry=TERM/30/KILL/5 \
                --pidfile $PIDFILE 
            }

            case "$1" in
              start)
                log_daemon_msg "Starting $DESC" 
                start
                case "$?" in
                    0) log_end_msg 0 ;;
                    1) log_end_msg 1 ;;
                esac
                ;;
              stop)
                log_daemon_msg "Stopping $DESC"
                stop
                case "$?" in
                    0) log_end_msg 0 ;;
                    1) log_end_msg 1 ;;
                esac
                ;;
              restart|force-reload)
                log_daemon_msg "Restarting $DESC" 
                stop
                case "$?" in
                  0|1)
                    start
                    case "$?" in
                        0) log_end_msg 0 ;;
                        1) log_end_msg 1 ;; 
                    esac
                    ;;
                esac
                ;;
                status)
                status_of_proc "$DAEMON" "$NAME"  
                ;;
              *)
                echo "Usage: $SCRIPTNAME {start|stop|status|restart|force-reload}" 
                ;;
            esac
            exit 0
            ### END OF SCRIPT

    1. use the script

            # 1. save to /etc/init.d
            $ chmod +x mrtg
            $ sudo mv mrtg /etc/init.d/

            # 2. link mrtg script to
            # all of multi-user run levels (2-5)
            $ sudo update-rd.d mrtg defaults
            # remove script from multi-user run levels
            $ sudo update-rc.d -f mrtg remove

            # 3. start mrtg use this script
            $ sudo service mrtg start

1. [nagios core 4 installation on ubuntu 12.04](https://raymii.org/s/tutorials/Nagios_Core_4_Installation_on_Ubuntu_12.04.html)

    1. copy the included config from nagios

            $ cd ~/Downloads/nagioscore-nagios-4.0.8/sample-config
            $ cp mrtg.cfg /usr/local/nagios/etc/

    1. create a folder for the graphs and files

            $ mkdir -p /usr/local/nagios/share/stats

    1. config mrtg to use this folder

            $ sudo pico /usr/local/nagios/etc/mrtg.cfg
            # add the following at the top of the file
            WorkDir: /usr/local/nagios/share/stats

    1. do the initial run

            $ sudo env LANG=C /usr/bin/mrtg /usr/local/nagios/etc/mrtg.cfg

    1. create the html pages

            $ sudo /usr/bin/indexmaker /usr/local/nagios/etc/mrtg.cfg \
            --output=/usr/local/nagios/share/stats/index.html

    1. create a cron job to run mrtg every 5 minutes

            $ sudo pico /etc/cron.d/mrtg-nagios
            */5 * * * * root env LANG=C /usr/bin/mrtg /usr/local/nagios/etc/mrtg.cfg

    1. browser `http://node5/nagios/stats`

    1. add link to nagios menu

            $ sudo pico /usr/local/nagios/share/side.php
            <div class="navsection">
                <div class="navsectiontitle">Extra Tools</div>
                    <div class="navsectionlinks">
                        <ul class="navsectionlinks">
                            <li><a href="/nagios/stats" target="<?php echo $link_target;?>">MRTG stats</a></li>
                        </ul>
                    </div>
                </div>
            </div>

### references

1. [linux mrtg configuration](http://www.cyberciti.biz/nixcraft/linux/docs/uniqlinuxfeatures/mrtg/mrtgconifg.php)

1. [mrtg installation and configuration in debian based distributions 2](http://www.debianadmin.com/mrtg-installation-and-configuration-in-debian-based-distributions-2.html)

1. [MRTG - The Multi Router Traffic Grapher](https://help.ubuntu.com/community/MRTG)
