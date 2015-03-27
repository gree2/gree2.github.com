---
layout: post
title: "erase disk and reinstall mac os x yosemite"
description: ""
category: [mac]
tags: [filevault, erase disk, reinstall, os x, yosemite]
---
{% include JB/setup %}

### problem

`can not turn off filevault`

	$ diskutil cs list

	1. check status
	$ fdesetup status

	2. enable filevault
	$ sudo fdesetup enable

	3. disable filevault
	$ sudo fdesetup disable

### solution

`i do want a fresh start!!!`

`erase all data`

`reinstall mac os x yosemite`

1. backup your essential files

1. connect to the internet

1. restart you computer 

	* then hold `command+r` keys while the computer restart

1. choose `disk utility` and then `continue`

1. `right click` your `startup disk`

1. `unlock` your `startup disk`

1. click `erase` tab

	* choose `mac os extended(journaled)` format

	* type a name for your disk

	* click `erase`

1. after erased quit `disk utility`

1. choose `reinstall mac os x` click `continue`

1. follow the instructions