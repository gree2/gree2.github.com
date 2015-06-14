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

### syntax

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

### usage

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