---
layout: post
title: "disable windows ports"
description: ""
category: [windows]
tags: [ports, 135, 139, 445]
---
{% include JB/setup %}


### steps

1. disable 135

	1. [solution 1](http://ssj100.fullsubject.com/t181-how-to-disable-ports-135-137-139-445-windows-xp) failed

			# 1.
			> regedit

			# 2.
			HKEY_LOCAL_MACHINE\Software\Microsoft\OLE
			"EnableDCOM" "Y" ==> "N"

			# 3.
			HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\RPC
			"DCOM Protocols" "ncacn_ip_tcp" ==> ""

1. disable 137, 138, 139

			# 1.
			My Network Places --> Properties

			# 2.
			Local Area Connection --> Properties

			# 3.
			Internet Protocol (TCP/IP) --> Properties

			# 4.
			Advanced.. --> WINS tab

			# 5.
			NetBIOS setting ==> Disable NetBIOS over TCP/IP ==> accept

1. disable 445

	1. [solution 1](http://ssj100.fullsubject.com/t181-how-to-disable-ports-135-137-139-445-windows-xp) failed

			# 1.
			> regedit

			# 2. add registry key
			HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NetBT\Parameters

			# 3.
			Name: SMBDeviceEnabled
			Type: DWORD (REG_DWORD)
			Data: 0

			# 4.
			reboot

	1. [solution 2](https://superuser.com/questions/629648/how-to-disable-feature-that-opened-port-445-on-windows-server) passed

			# 1.
			> sc stop lanmanserver

			# 2.
			> sc config lanmanserver start=disabled

			# 3.
			> shutdown /r /t 0

			# 4. netstat -an | findstr 445

### reference

1. [http://ssj100.fullsubject.com/t181-how-to-disable-ports-135-137-139-445-windows-xp](http://ssj100.fullsubject.com/t181-how-to-disable-ports-135-137-139-445-windows-xp)

1. [https://superuser.com/questions/629648/how-to-disable-feature-that-opened-port-445-on-windows-server](https://superuser.com/questions/629648/how-to-disable-feature-that-opened-port-445-on-windows-server)
