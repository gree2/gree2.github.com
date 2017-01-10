---
layout: post
title: "python send html mail"
description: ""
category: [python]
tags: [send, html, mail]
---
{% include JB/setup %}


### code

            #! coding: utf-8
            """send email"""

            import smtplib
            from email.MIMEMultipart import MIMEMultipart
            from email.MIMEText import MIMEText

            mail_from = "hqlgree2@gmail.com"
            mail_recv = [mail_from] 
            mail_smtp = "smtp.gmail.com" 
            mail_pass = "************" 

            html = """<font color=#437C17><h1>this is first line</h1></font>
            <font color=#7D0552><h2>this is first line</h2></font>
            <font color=#05157D><h3>this is first line</h3></font>
            <font color=#812DA5><h4>this is first line</h4></font>
            <font color=#2DA681><h5>this is first line</h5></font>
            <font color=#A62D79><h6>this is first line</h6></font>"""


            def send_html(mail_recv, sub, text, html):
                """send_html"""
                msg = MIMEMultipart('alternative')
                msg['Subject'] = sub
                msg['From'] = mail_from
                msg['To'] = ";".join(mail_recv)

                # Record the MIME types of both parts - text/plain and text/html.
                part_text = MIMEText(text, 'plain')
                part_html = MIMEText(html, 'html')

                # Attach parts into message container.
                # According to RFC 2046, the last part of a multipart message, in this case
                # the HTML message, is best and preferred.
                msg.attach(part_text)
                msg.attach(part_html)

                smtp = smtplib.SMTP(mail_smtp)
                smtp.login(mail_from, mail_pass)
                smtp.sendmail(mail_from, mail_recv, msg.as_string())
                smtp.quit()
                print 'send'

            send_html(mail_recv, 'subject', 'this is text part', html)
