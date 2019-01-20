---
layout: post
title: "config h3c router from macos"
description: ""
category:
tags: [console, terminal, screen, usbserial, serial, cu]
---
{% include JB/setup %}


### steps

1. install ft2232d driver

1. plug in usb console line

1. list devices in /dev

        $ cd /dev
        $ ls | grep usb
        cu.usbserial-A5066QR4

1. use screen command connect to router console

        $ screen /dev/cu.usbserial-A5066QR4 9600
        # hit enther key
        H3C>

### references

1. screen

        $ screen ls

        $ screen -r 16582
