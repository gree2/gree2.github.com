---
layout: post
title: "find printers with nmap"
description: ""
category: [command]
tags: [maxos, printer, nmap]
---
{% include JB/setup %}


### usages

1. use nmap

		$ nmap -p 515,631,9100 -oG - 192.168.200.1/24
		Host: 192.168.200.131 ()	Status: Up
		Host: 192.168.200.131 ()	Ports: 515/open/tcp//printer///, 631/closed/tcp//ipp///, 9100/open/tcp//jetdirect///

1. references

	1. [https://serverfault.com/questions/154650/find-printers-with-nmap](https://serverfault.com/questions/154650/find-printers-with-nmap)
