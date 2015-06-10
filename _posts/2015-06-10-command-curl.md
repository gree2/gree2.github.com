---
layout: post
title: "command curl"
description: ""
category: [command]
tags: [curl, transfer, tool, library]
---
{% include JB/setup %}


### info

1. command line tool and library for transferring data with url syntax

1. supporting

            dict, file, ftp, ftps, gopher,
            http, https, imap, imaps, ldap,
            ldaps, pop3, pop3s, rtmp, rtsp,
            scp, sftp, smb, smtp, smtps, telnet, tftp

1. supports ssl certificates

1. uploading

            http post, http put,
            ftp, http form based

1. proxies, http/2, cookies, user+password authentication

            basic, plain, digest, cram-md5, ntlm, negotiate and kerberos

1. file transfer resume

1. proxy tunneling

1. used for

            in cars, television sets, routers, printers,
            audio equipment, mobile phones, tablets, settop boxes, media players

### demo

1. http scripting

    1. show everything curl sends and receives

            $ curl --trace-ascii debug.txt http://www.baidu.com/

    1. see timing

            $ curl --trace-ascii debug.txt --trace-time http://www.baidu.com/

1. url

    1. point out a different ip for a `host` name

            $ curl --resolve www.baidu.com:80:127.0.0.1 http://www.baidu.com/

    1. specify the `port` number in the url

            $ curl http://www.baidu.com:1234/

            # specify proxy's port number
            $ curl --proxy http://proxy.com:4321 http://www.baidu.com/

    1. username and password

            $ curl http://username:password@baidu.com/

            $ curl -u username:password http://baidu.com/

1. fetch a page

    1. get

            $ curl http://www.baidu.com

    1. multiple urls

            $ curl http://www.baidu.com http://www.google.com

            # send two posts
            curl --data name=curl http://www.baidu.com http://www.google.com

    1. multiple http methods

            # send first a head
            # send second a get
            $ curl -l http://www.baidu.com --next http://www.google.com

            # send first a post
            # send second a get
            curl -d score=10 http://example.com/post.cgi --next http://example.com/results.html

1. html forms

    1. get

            <form method="GET" action="junk.cgi">
            <input type=text name="birthyear">
            <input type=submit name=press value="OK">
            </form>

            $ curl "http://www.hotmail.com/when/junk.cgi?birthyear=1905&press=OK"

    1. post

            <form method="POST" action="junk.cgi">
            <input type=text name="birthyear">
            <input type=submit name=press value=" OK ">
            </form>

            $ curl --data "birthday=1905&press=%20OK%20" http://www.example.com/when.cgi

    1. file upload post

            <form method="POST" enctype='multipart/form-data' action="upload.cgi">
            <input type=file name=upload>
            <input type=submit name=press value="OK">
            </form>

            $ curl --form upload=@localfilename --form press=OK url

    1. hidden fields

            <form method="POST" action="foobar.cgi">
            <input type=text name="birthyear">
            <input type=hidden name="person" value="daniel">
            <input type=submit name="press" value="OK">
            </form>

            $ curl --data "birthday=1905&press=OK&person=daniel" url

1. http upload

    1. put

            $ curl --upload-file uploadfile http://www.example.com/receive.cgi

1. http authentication

    1. basic authentication

            $ curl --user username:password http://www.example.com

    1. other authentication

    1. proxy authentication

            $ curl --proxy-user proxyuser:password curl.haxx.se

1. more http headers

    1. referer (misspelled)

            $ curl --referer http://www.example.com http://www.example.com

    1. user agent

            # ie 5 on a Windows 2000 box:
            $ curl --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" url

            # Netscape 4.73 on an old Linux box:
            curl --user-agent "Mozilla/4.73 [en] (X11; U; Linux 2.2.15 i686)" url

1. redirects

    1. location header

            # tell curl to follow a location
            $ curl --location http://www.example.com

1. cookies

    1. cookies options

            $ curl --cookies "name=daniel" http://example.com

            # record cookies
            $ curl --dump-header header_and_cookies http://www.example.com

            # use previously stored cookies
            $ curl --cookies stored_cookies_in_file http://www.example.com

            # want curl to understand received cookies
            $ curl --cookies nada --location http://www.example.com

            # read and write cookies files
            $ curl --cookies cookies.txt --cookies-jar newcookies.txt http://www.example.com

1. https

    1. https is http secure

            $ curl https://secure.example.com

    1. certificates

            $ curl --cert mycert.pem https://secure.example.com

            $ curl --cert ca-bundle.pem https://example.com

1. custom request elements

    1. modify method and headers

            $ curl --data "<xml>" --header "Content-Type: text/xml" --request PROPFIND url.com

            # delete a default header by providing one without content
            $ curl --header "Host:" http://www.example.com

            # add header the same way
            $ curl --header "Destination: http://nowhere" http://example.com