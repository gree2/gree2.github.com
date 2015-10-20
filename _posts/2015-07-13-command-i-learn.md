---
layout: post
title: "command i learn"
description: ""
category: [command]
tags: [sysctl, wget, grep]
---
{% include JB/setup %}


### commands

1. apt-get

    1. install

            $ sudo apt-get install mysql-server mysql-client
            E: dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem. 
            $ sudo dpkg --configure -a
            Setting up libmysqlclient18:amd64 (5.6.24-0ubuntu2) ...
            Setting up libterm-readkey-perl (2.32-1build1) ...
            Setting up libaio1:amd64 (0.3.110-1) ...
            Setting up libdbi-perl (1.631-3build1) ...
            Setting up mysql-client-core-5.6 (5.6.24-0ubuntu2) ...
            Setting up mysql-server-core-5.6 (5.6.24-0ubuntu2) ...
            Setting up libdbd-mysql-perl (4.028-2) ...
            Setting up mysql-client-5.6 (5.6.24-0ubuntu2) ...
            Processing triggers for libc-bin (2.21-0ubuntu4) ...

1. curl 

    1. [curl command resume broken download](http://www.cyberciti.biz/faq/curl-command-resume-broken-download/)

            # 1. auto find out where/how to resume the transfer
            $ curl -L -o 'filename' -C - url

            # 2. continue a previous file transfer at the given offset
            $ curl -C offset url

1. grep

    1. [find command that returns line number of the string](http://stackoverflow.com/questions/7600313/find-command-that-returns-line-number-of-the-string)

            $ grep -n 'search-term' *.c

    1. [finding all files containing a text string on linux](http://stackoverflow.com/questions/16956810/finding-all-files-containing-a-text-string-on-linux)

            $ grep -rnw _post -e "retry policy"
            _posts//2015-07-06-hadoop-cluster-5-node-setup.md:1068:            2015-07-08 08:20:48,046 INFO org.apache.hadoop.ipc.Client: Retrying connect to server: node5/192.168.120.155:9000. Already tried 3 time(s); retry policy is RetryUpToMaximumCountWithFixedSleep(maxRetries=10, sleepTime=1000 MILLISECONDS)

    1. demo

            # 1. find all `java` in all `sh` files
            $ grep "java" *.sh

            # 2. search all subdirectories recursively
            $ grep -r "java" .
            $ grep -R "java" .

            # 3. only display filenames
            $ grep -H -r "java" .

            # 4. use `cut`
            $ grep -H -R "java" . | cut -d: -f1

            # 5. display only words
            $ grep -w -R "java" *.sh

            # 6. search two or more words
            $ grep -w -R "java|then" *.sh

1. [hardware info](http://superuser.com/questions/303981/what-is-the-linux-command-to-find-out-hardware-info) and [pixelbeat](http://www.pixelbeat.org/cmdline.html) and [cyberciti](http://www.cyberciti.biz/faq/linux-command-to-find-the-system-configuration-and-hardware-information/)

    1. kernel version and system architecture

            $ uname -mrs
            $ uname -a

    1. name and version of distribution

            $ head -n1 /etc/issues

    1. disk

            # 1. all partitions registered on the system
            $ cat /proc/partitions

            # 2. info about disk sda
            $ hdparm -i /dev/sda

            # 3. dispaly installed hard disk and size
            $ fdisk -l | grep '^Disk /dev/'

            # 4. 
            $ df -lh
            Filesystem      Size  Used Avail Use% Mounted on
            /dev/sda3       611G  2.0G  578G   1% /
            tmpfs            16G     0   16G   0% /dev/shm
            /dev/sda1        31G  205M   29G   1% /boot

    1. ram total seen by system

            $ grep MemTotal /proc/meminfo

    1. cpu info

            $ grep "model name" /proc/cpuinfo
            $ less /proc/cpuinfo
            # lscpu

    1. list all usb devices

            $ lsusb

    1. list all block devices

            $ lsblk

    1. display info about hardware raid

            $ arcconf getconfig DEV
            # /usr/StorMan/arcconf getconfig 1

    1. dump all hardware info

            # motherboard/cpu/verdor/serial-numbers
            # ram/disks/other info
            $ dmidecode | less

    1. using [lspci](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-sysinfo-hardware.html)

            $ lspci
            $ lspci -v
            $ lspci -vv

1. [ifconfig](http://www.tecmint.com/ifconfig-command-examples/)

    1. display all network interfaces

            $ ifconfig -a

    1. specific interface

            $ ifconfig eth0

    1. enable/disable interface

            $ ifconfig eth0 up
            $ ifup eth0

            $ ifconfig eth0 down
            $ ifdown eth0

    1. assign ip

            $ ifconfig eth0 192.168.100.100

    1. assign netmask

            $ ifconfig eth0 netmask 255.255.255.0

    1. assign broadcast

            $ ifconfig eth0 broadcast 192.168.100.255

    1. assign ip netmask broadcast

            $ ifconfig eth0 192.168.100.100 netmask 255.255.255.0 broadcast 192.168.100.255

    1. change mtu

            $ ifconfig eth0 mtu 1000

    1. enable/disable promiscuout mode

            $ ifconfig eth0 promisc
            $ ifconfig eth0 -promisc

    1. add alias to interface

            $ ifconfig eth0:0 192.168.100.100
            $ ifconfig eth0:0

    1. remove alias

            $ ifconfig eth0:0 down

    1. change mac address

            $ ifconfig eth0 hw ether AA:BB:CC:DD:EE:FF

1. line edit shortcut

    1. [how do i clear delete the current line in terminal](http://stackoverflow.com/questions/9679776/how-do-i-clear-delete-the-current-line-in-terminal)

            $ some command you input_ and after the cursor
                                    ^
                                    |
                                    ctrl + u delete all before
                                    ctrl + k delete all after
                                    ctrl + c cancel
                                    ctrl + y undo
                                    ctrl + w delete a word
                                    ctrl + l clear entire prompt

1. locale

    1. [set locale zh_CN.UTF-8](http://askubuntu.com/questions/162391/how-do-i-fix-my-locale-issue/229512#229512)

            $ sudo locale-gen zh_CN.UTF-8
            $ sudo dpkg-reconfigure locales

    1. [ssh server](https://wiki.debian.org/Locale)

            $ sudo pico /etc/ssh/sshd_config
            # conment out this line
            # AcceptEnv LANG LC_*

            # restart ssh
            $ /etc/init.d/ssh restart

    1. [ssh client](https://wiki.debian.org/Locale)

            $ sudo pico /etc/ssh/ssh_config
            # conment out this line
            # SendEnv LANG LC_*

    1. [fixing locale warnings notices issues on linux server or desktop](http://codetheory.in/fixing-locale-warnings-notices-issues-on-linux-server-or-desktop/)

            $ locale
            locale: Cannot set LC_CTYPE to default locale: No such file or directory
            locale: Cannot set LC_MESSAGES to default locale: No such file or directory
            locale: Cannot set LC_ALL to default locale: No such file or directory
            LANG=en_US
            LANGUAGE=
            LC_CTYPE="en_US"
            LC_NUMERIC=en_US.UTF-8
            LC_TIME=en_US.UTF-8
            LC_COLLATE="en_US"
            LC_MONETARY=en_US.UTF-8
            LC_MESSAGES="en_US"
            LC_PAPER=en_US.UTF-8
            LC_NAME=en_US.UTF-8
            LC_ADDRESS=en_US.UTF-8
            LC_TELEPHONE=en_US.UTF-8
            LC_MEASUREMENT=en_US.UTF-8
            LC_IDENTIFICATION=en_US.UTF-8
            LC_ALL=

            $ sudo update-locale LC_ALL="en_US.UTF-8"
            $ logout
            # login
            $ locale
            LANG=en_US.UTF-8
            LANGUAGE=
            LC_CTYPE="en_US.UTF-8"
            LC_NUMERIC="en_US.UTF-8"
            LC_TIME="en_US.UTF-8"
            LC_COLLATE="en_US.UTF-8"
            LC_MONETARY="en_US.UTF-8"
            LC_MESSAGES="en_US.UTF-8"
            LC_PAPER="en_US.UTF-8"
            LC_NAME="en_US.UTF-8"
            LC_ADDRESS="en_US.UTF-8"
            LC_TELEPHONE="en_US.UTF-8"
            LC_MEASUREMENT="en_US.UTF-8"
            LC_IDENTIFICATION="en_US.UTF-8"
            LC_ALL=en_US.UTF-8

            $ cat /etc/default/locale 
            LANG="en_US.UTF-8"
            LC_NUMERIC="en_US.UTF-8"
            LC_TIME="en_US.UTF-8"
            LC_MONETARY="en_US.UTF-8"
            LC_PAPER="en_US.UTF-8"
            LC_NAME="en_US.UTF-8"
            LC_ADDRESS="en_US.UTF-8"
            LC_TELEPHONE="en_US.UTF-8"
            LC_MEASUREMENT="en_US.UTF-8"
            LC_IDENTIFICATION="en_US.UTF-8"

1. netstat

    1. [nagios connection refused socket timeout](http://stackoverflow.com/questions/12088830/nagios-connection-refused-socket-timeout)

            # check host is listening on port 5666
            > netstat -aon | findstr 5666

    1. get all tcp process pid port

            # get `pid/program name` from port
            $ netstat -antp
            (Not all processes could be identified, non-owned process info
             will not be shown, you would have to be root to see it all.)
            Active Internet connections (servers and established)
            Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
            tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -               
            tcp        0      0 10.13.178.100:22        10.13.178.171:61212     ESTABLISHED -               
            tcp6       0      0 :::9092                 :::*                    LISTEN      1288/java       
            tcp6       0      0 127.0.0.1:8005          :::*                    LISTEN      1288/java       
            tcp6       0      0 :::8009                 :::*                    LISTEN      1288/java       
            tcp6       0      0 :::8080                 :::*                    LISTEN      1288/java       
            tcp6       0      0 :::22                   :::*                    LISTEN      -

1. openssl

    1. dgst

            $ echo -n "http://releases.ubuntu.com/14.04/ubuntu-14.04.1-server-i386.iso" | openssl dgst
            (stdin)= 412e4a818779276896b68b3ba0913534

    1. dgst -sha256

            $ echo -n "http://releases.ubuntu.com/14.04/ubuntu-14.04.1-server-i386.iso" | openssl dgst -sha256
            (stdin)= 90a2489db0fb9bf8e4062a05cdd528d372c3ff6e028edfde5666b7ae73a3d18a

1. ps

    1. filter java process

            $ ps -ef | grep java
            node      1288     1  2 14:52 pts/0    00:00:15 /usr/local/java/jdk1.7.0_79/jre/bin/java -Djava.util.logging.config.file=/home/node/biserver-ce/tomcat/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Xms1024m -Xmx2048m -XX:MaxPermSize=256m -Dsun.rmi.dgc.client.gcInterval=3600000 -Dsun.rmi.dgc.server.gcInterval=3600000 -Djava.endorsed.dirs=/home/node/biserver-ce/tomcat/endorsed -classpath /home/node/biserver-ce/tomcat/bin/bootstrap.jar -Dcatalina.base=/home/node/biserver-ce/tomcat -Dcatalina.home=/home/node/biserver-ce/tomcat -Djava.io.tmpdir=/home/node/biserver-ce/tomcat/temp org.apache.catalina.startup.Bootstrap start
            node      1366  1102  0 15:00 pts/0    00:00:00 grep --color=auto java

1. ssh

    1. [ssh-keyscan](http://askubuntu.com/questions/123072/ssh-automatically-accept-keys)

            # 1. if not in `known_hosts` you must confirm `yes`
            $ start-dfs.sh
            Starting namenodes on [singlenode]
            The authenticity of host 'singlenode (192.168.100.100)' can't be established.
            ECDSA key fingerprint is f1:14:06:ef:f4:08:e5:a2:bd:c8:54:50:1d:ca:8e:56.
            Are you sure you want to continue connecting (yes/no)? yes
            singlenode: Warning: Permanently added 'singlenode,192.168.100.100' (ECDSA) to the list of known hosts.
            singlenode: starting namenode, logging to /opt/bigdata/hadoop-2.7.0/logs/hadoop-hadoop-namenode-singlenode.out
            singlenode: starting datanode, logging to /opt/bigdata/hadoop-2.7.0/logs/hadoop-hadoop-datanode-singlenode.out
            Starting secondary namenodes [0.0.0.0]
            The authenticity of host '0.0.0.0 (0.0.0.0)' can't be established.
            ECDSA key fingerprint is f1:14:06:ef:f4:08:e5:a2:bd:c8:54:50:1d:ca:8e:56.
            Are you sure you want to continue connecting (yes/no)? yes
            0.0.0.0: Warning: Permanently added '0.0.0.0' (ECDSA) to the list of known hosts.
            0.0.0.0: starting secondarynamenode, logging to /opt/bigdata/hadoop-2.7.0/logs/hadoop-hadoop-secondarynamenode-singlenode.out

            # 2. stop `dfs` and rm `known_hosts`
            $ stop-dfs.sh
            $ rm known_hosts

            # 3. add to `known_hosts`
            $ ssh-keyscan -H 0.0.0.0 >> known_hosts
            $ ssh-keyscan -H singlenode >> known_hosts
            $ ssh-keyscan -H 192.168.100.100 >> known_hosts

            # 4. `start-dfs`
            Starting namenodes on [singlenode]
            singlenode: starting namenode, logging to /opt/bigdata/hadoop-2.7.0/logs/hadoop-hadoop-namenode-singlenode.out
            singlenode: starting datanode, logging to /opt/bigdata/hadoop-2.7.0/logs/hadoop-hadoop-datanode-singlenode.out
            Starting secondary namenodes [0.0.0.0]
            0.0.0.0: starting secondarynamenode, logging to /opt/bigdata/hadoop-2.7.0/logs/hadoop-hadoop-secondarynamenode-singlenode.out

1. sudo

    1. [disable ipv6](http://askubuntu.com/questions/440649/how-to-disable-ipv6-in-ubuntu-14-04)

            # 1. append to `/etc/sysctl.conf`
            $ sudo sh -c 'echo "net.ipv6.conf.all.disable_ipv6 = 1" >> /etc/sysctl.conf'
            $ sudo sh -c 'echo "net.ipv6.conf.default.disable_ipv6 = 1" >> /etc/sysctl.conf'
            $ sudo sh -c 'echo "net.ipv6.conf.lo.disable_ipv6 = 1" >> /etc/sysctl.conf'

            # 2. verify edit
            $ sudo sysctl -p
            net.ipv6.conf.all.disable_ipv6 = 1
            net.ipv6.conf.default.disable_ipv6 = 1
            net.ipv6.conf.lo.disable_ipv6 = 1

            # 3. verify
            $ cat /proc/sys/net/ipv6/conf/all/disable_ipv6
            1

1. sysctl

    1. [ip_local_port_range]((http://zookeeper-user.578899.n2.nabble.com/Zookeeper-listening-to-ports-other-than-clientPort-and-server-x-ports-td7580137.html#a7580138))

            $ sysctl net.ipv4.ip_local_port_range
            net.ipv4.ip_local_port_range = 32768    61000

1. telnet

    1. [how do i exit telnet](http://superuser.com/questions/486496/how-do-i-exit-telnet)

            $ telnet node1 5666
            Trying 192.168.120.151...
            Connected to node1.
            Escape character is '^]'.
            ctrl+]
            telnet> quit

1. tr

    1. [squeeze spaces](http://stackoverflow.com/questions/7142735/linux-cut-help-how-to-specify-more-spaces-for-the-delimiter)

            $ fab sete hd1 | grep part-m-00000 | tr -s ' ' | cut -f 10 -d ' '
            # get hdfs dfs -ls -R output's file path

1. tail

    1. [how can i reverse the order of lines in a file](http://stackoverflow.com/questions/742466/how-can-i-reverse-the-order-of-lines-in-a-file)

            $ tail -r myfile.txt

    1. [how reverse selected lines order in vim](http://superuser.com/questions/189947/how-reverse-selected-lines-order-in-vim)

            :g/^/m0

            :help 12.4

1. wget

    1. [spider a website and return urls only](http://stackoverflow.com/questions/2804467/spider-a-website-and-return-urls-only)

            wget --spider --force-html -r -l2 http://mirrors.cnnic.cn/apache/ 2>&1 \
            | grep '^--' | awk '{ print $3 }' \
            | grep -v '\.\(css\|js\|png\|gif\|jpg\)$' \
            > urls.m3u
