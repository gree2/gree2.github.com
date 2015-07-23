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

1. key

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
