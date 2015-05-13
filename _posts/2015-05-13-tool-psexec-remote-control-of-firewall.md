---
layout: post
title: "tool psexec remote control of firewall"
description: ""
category: [tool]
tags: [psexec, cmd, netsh]
---
{% include JB/setup %}


### solution

1. download [PsTools](https://technet.microsoft.com/en-us/sysinternals/bb896649.aspx) use this [link](https://download.sysinternals.com/files/PSTools.zip)

1. unzip `PsTools.zip` to a folder

1. add `path/to/PsTools` to path

1. connect to remote server's `cmd`

            E:\> psexec \\192.168.100.100 -u user -p password cmd
            PsExec v2.11 - Execute processes remotely
            Copyright (C) 2001-2014 Mark Russinovich
            Sysinternals - www.sysinternals.com

            Starting PSEXESVC service on 192.168.100.100...

            Microsoft Windows [Version 6.1.7601]
            ...

            C:\Windows\system32>

1. `disable` firewall

            C:\Windows\system32> netsh firewall set opmode enable

1. `enable` firewall

            C:\Windows\system32> netsh firewall set opmode disable

1. exit remote `cmd`

            C:\Windows\system32> exit
            cmd exited on 192.168.100.100 with error code 0.
            E:\>