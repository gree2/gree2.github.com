---
layout: post
title: "python retrieve mail"
description: ""
category: [python]
tags: [poplib, email, text/plain, text/html, multipart, alternative]
---
{% include JB/setup %}


### code

            #! coding: utf-8
            import poplib
            import email
            from email.parser import Parser

            mail_pop3 = 'pop3.163.com'
            mail_user = 'hqlgree2@gmail.com'
            mail_pass = '************'

            server = poplib.POP3(mail_pop3)
            # debug info
            # server.set_debuglevel(1)
            # print(server.getwelcome())
            server.user(mail_user)
            server.pass_(mail_pass)

            print "=" * 50
            print('messages: %s size: %s' % server.stat())
            print "=" * 50
            resp, mails, octets = server.list()
            # print(mails)
            # count form 1
            index = len(mails)
            resp, lines, octets = server.retr(index)
            content = '\r\n'.join(lines)
            msg = Parser().parsestr(content)
            for part in msg.walk():
                content_type = part.get_content_type()
                if 'multipart/alternative' == content_type:
                    continue
                print content_type
                print part.get_payload()
                print "-" * 50
            # delete mail
            # server.dele(index)
            server.quit()
