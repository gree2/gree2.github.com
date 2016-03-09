---
layout: post
title: "ntp server activation on windows 7"
description: ""
category: [windows]
tags: [windows 7, ntp, ntpclient, ntpserver, w32tm, w32time]
---
{% include JB/setup %}


### [ntp server activation](http://www.skylark.tv/support/ntp_server_activation/)

1. set `Enabled` to `1`


			# 1. win+r
			# 2. type regedit
			HKLM\System\CurrentControlSet\services\W32Time\TimeProviders\NtpServer
			# 3. set `Enabled` parameter as `1`

1. restart time service

			# run `cmd.ext` as administrator
			> net stop w32time && net start w32time

1. test

			> w32tm /query /configuration

### main settings of the ntp server

1. `HKLM\System\CurrentControlSet\services\W32Time\Config`

1. `HKLM\System\CurrentControlSet\services\W32Time\Parameters`

1. `HKLM\System\CurrentControlSet\services\W32Time\TimeProviders\NtpClient`