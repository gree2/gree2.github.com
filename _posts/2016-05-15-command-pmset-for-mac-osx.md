---
layout: post
title: "command pmset for mac osx"
description: ""
category: [command]
tags: [sleep, pmset, sleepnow, displaysleepnow]
---
{% include JB/setup %}


### problem

1. put my mac to sleep when my wife is away

1. this is done on my thinkpad t430

	1. steps

			$ ssh gree2@my.mac.ip.address
			$ pmset sleepnow

1. [pmset manual](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/pmset.1.html)

	1. pmset [touch | sleepnow | displaysleepnow | lock | boot]

			sleepnow - causes an immediate system sleep
			displaysleepnow - causes display to go to sleep immediately