---
layout: post
title: "batch add users and groups on windows"
description: ""
category: [batch]
tags: [user, group]
---
{% include JB/setup %}

### user management

1. add a local user

		net user username password /add

1. delete a local user

		net user username /delete

1. add users from a .csv file

	* csv file content

			username1,password1
			username2,password2
			username3,password3

	* batch

			rem add users from a .csv file
			@echo off
			FOR /F "token=1,2 delims=," %%a IN (users.csv) DO net user %1 %1 /add

			rem delete .csv file
			del /F /Q users.csv

### group management

1. add a local group

		net localgroup groupname /Comment:"group comment" /add

1. delete a local group

		net localgroup groupname /delete

1. add a user to a local group

		net localgroup groupname username /add

1. delete a user form a local group

		net localgroup groupname username /delete

### managing a local administrator account in workgroup environments

		@echo off
		rem add the "ITAdmin" user
		net user ITAdmin %1 /add
		rem add the user to "administrators" group
		net localgroup administrators ITAdmin /add
		rem remove the user from the default "users" group
		net localgroup users ITAdmin /delete

### reference

[https://community.landesk.com/support/docs/DOC-2413](https://community.landesk.com/support/docs/DOC-2413)