---
layout: post
title: "command nc"
description: ""
category: [command]
tags: [netcat, nc, network]
---
{% include JB/setup %}


### info

1. reads and writes data across network connections using tcp or udp

    1. simple tcp proxies

    1. shell-script based http clients and servers

    1. network daemon testing

    1. a socks or http proxyCommand for ssh

### [syntax](http://www.computerhope.com/unix/nc.htm)

1. nc

            -46bCDdhklnrStUuvZz 
            -I length 
            -i interval 
            -O length 

            -P proxy_username 
            -p source_port 
            -q seconds 
            -s source 

            -T toskeyword 
            -V rtable 
            -w timeout 
            -X proxy_protocol 

            -x proxy_address:port
            destination port

### [usage](http://mylinuxbook.com/linux-netcat-command/)

1. client/server model

    1. on one console start `nc` listening on a special port for connection

            $ nc -l 1234

    1. on a second console or machine connect to the machine and port

            $ nc 127.0.0.1 1234

            # terminated using an eof (ctrl+d)

1. data transfer

    1. start `nc` to listen on a special port with output captured into a file

            $ nc -l 1234 > filename.out

    1. connect to the listening `nc` process

            # feeding it the file which is to be transferred
            $ nc 127.0.0.1 1234 < filename.in

1. talking to servers

    1. retrieve home page form a website

            $ printf "GET / HTTP/1.0\r\n\r\n" | nc host.example.com 80

    1. an email may be submitted to an smtp server using

            $ nc -C localhost 25 << EOF
            HELO host.example.com
            MAIL FROM:<user1@host.example.com>
            RCPT TO:<user2@host.example.com>
            DATA
            body of email
            .
            QUIT
            EOF

1. port scanning

    1. `-z` tell `nc` to report open ports

            $ nc -zv host.example.com 20-30

    1. specify a list of ports to scan

            $ nc -zv host.example.com 20 22 80

    1. find out which server software is running

            $ echo "QUIT" | nc host.example.com 20-30

1. directory transfer

    1. use tar 

            # 1. server
            $ tar -cvf - dir_name | nc -l 1567

            # 2. client
            $ nc -n localhost 1567 | tar -xvf -

    1. usr tar and compress

            # 1. server
            $ tar -cvf - dir_name | bzip2 -z | nc -l 1567

            # 2. client
            $ nc -n localhost 1567 | bzip2 -d | tar -xvf -

1. encrypt data when sending over the network

    1. server

            $ nc localhost 1567 | mcrypt -flush -bare -F -q -d -m ecb > file.txt

    1. client

            $ mcrypt -flush -bare -F -q -m ecb < file.txt | nc -l 1567

1. stream a video

    1. server

            $ cat video.avi | nc -l 1567

    1. nc localhost 1567 | mplayer -vo x11 -cache 3000 -

1. cloning a device

    1. server

            $ dd if=/dev/sda | nc -l 1567

    1. client

            $ nc -n localhost 1567 | dd of=/dev/sda

1. opening a shell

    1. support -c and -e option

            # 1. server
            $ nc -l 1567 -e /bin/bash -i

            # 2. client
            $ nc localhost 1567

    1. doesn't support -c or -e option (openbsd netcat)
       
            $ mkfifo /tmp/tmp_fifo
            $ cat /tmp/tmp_fifo | /bin/sh -i 2>&1 | nc -l 1567 > /tmp/tmp_fifo

            # 1. the input received from network is written to fifo file
            # 2.fifo file is read by cat and it content is send to sh command
            # 3. sh command processes the received input and write it back to netcat
            # 4. netcat send the output over the network to client

            # 2. client
            $ nc -n localhost 1567

1. reverse shell

    1. server

            $ nc -l 1567

    1. client

            $ nc localhost 1567 -e /bin/bash

1. specify source port

    1. server

            $ nc -l 1567

    1. client

            $ nc localhost 1567 -p 25
            # firewall filters all ports but 25
            # you need root permissions to use port less than 1024
            # this command will open port 25 at the client
            # which will be used for communication
            # otherwise any random port can be used

1. specify source address

    1. server

            $ nc -u -l 1567 < file.txt

    1. client

            $ nc -u localhost 1567 -s 192.168.100.100 > file.txt
            # have more than one addresses for your machine
            # want to explicitly tell which address to use for outgoing data
            # this command will bind the address 192.168.100.100

### demo

1. opens a tcp to port `42` using port `31337` as the source port

            $ nc -p 31337 -w 5 host.example.com 42

1. opens a udp connection to port 53 of host.example.com

            $ nc -u host.example.com 53

1. opens a tcp connection to port 42 using 10.1.2.3 as the ip for the local end of the connection

            $ nc -s 10.1.2.3 host.example.com 42

1. creates and listens on a unix-domain stream socket

            $ nc -lU /var/tmp/dsocket

1. connect to port 42 via an http proxy at 10.2.3.4 port 8080

            $ nc -x10.2.3.4:8080 -Xconnect host.example.com 42

1. same as the above example but enabling proxy authentication with username `ruser`

            $ nc -x10.2.3.4:8080 -Xconnect -Pruser host.example.com 42

1. [2015 05 24 apache flume getting started]({% post_url 2015-05-24-apache-flume-getting-started %})