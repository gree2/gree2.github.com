---
layout: post
title: "play with ansible"
description: ""
category: [auto]
tags: [ansible, ping]
---
{% include JB/setup %}


### ad-hoc demo

1. `hosts` inventory file


			$ pico hosts
			[local]
			127.0.0.1

1. `ping` demo

			$ ansible local -i hosts -m ping

1. `ansible.cfg` file

			$ pico ansible.cfg
			[defaults]
			hostfile=hosts
			remote_user=hqlgree2

1. do not need `-i`

			$ ansible local -m ping

			$ ansible local -m command -a uptime

			# command is the default module
			$ ansible local -a uptime

1. command contain space double quote it

			$ ansible local -a "tail /var/log/dmesg"

1. `sudo` as root use `-s` flag

			$ ansible local -s -a "tail /var/log/syslog"

1. install `nginx` on ubuntu

			$ ansible ubuntubox -s -m apt -a name=nginx

			# if fails
			$ ansible ubuntubox -s -m apt -a "name=nginx update_cache=yes"
