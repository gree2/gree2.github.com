---
layout: post
title: "introduction to ganglia on ubuntu"
description: ""
category: [ubuntu]
tags: [ganglia, gmond, gmetad, gweb, apache]
---
{% include JB/setup %}


### [introduction to ganglia on ubuntu 14.04](https://www.digitalocean.com/community/tutorials/introduction-to-ganglia-on-ubuntu-14-04)

1. master node

	1. install ganglia monitor, rrdtool, gmetad, ganglia web front end

			$ sudo apt-get install -y ganglia-monitor rrdtool gmetad ganglia-webfrontend

	1. config `site-enabled`

			$ sudo cp /etc/ganglia-webfrontend/apache.conf /etc/apache2/sites-enabled/ganglia.conf

	1. edit `gmetad` config

			$ sudo pico /etc/ganglia/gmetad.conf
			...
			data_source "hadoop_cluster" 60 localhost
			...

	1. edit `gmond` config

			$ sudo pico /etc/ganglia/gmond.conf
			...
			# 1. cluster
			cluster {
				# name from `gmetad.conf`
				name = "hadoop_cluster"
				owner = "unspecified"
				latlong = "unspecified"
				url = "unspecified"
			}
			...

			# 2. udp_send_channel
			udp_send_channel {
				# comment out
				# mcast_join = node5
				host = localhost
				port = 8649
				ttl = 1
			}
			...

			# 3. udp_recv_channel
			udp_recv_channel {
				# comment out
				# mcast_join = node5
				port = 8649
				# comment out
				# bind = node5
			}

			tcp_accept_channel {
				port = 8649
			}

	1. restart service

			$ sudo service ganglia-monitor restart && sudo service gmetad restart && sudo service apache2 restart

1. web interface browser [http://node5/ganglia](http://node5/ganglia)

1. client

	1. install

			$ sudo apt-get install -y ganglia-monitor

	1. edit `gmond` conf

			$ sudo pico /etc/ganglia/gmond.conf
			...
			# 1. cluster
			cluster {
				# cluster name
				name = "hadoop_cluster"
				owner = "unspecified"
				latlong = "unspecified"
				url = "unspecified"
			}
			...
			# 2. udp_send_channel
			udp_send_channel {
				# comment out
				# mcast_join = node5
				host = node5
				port = 8649
				ttl = 1
			}
			# 3. udp_recv_channel
			/*
			udp_recv_channel {
				mcast_join = node5
				port = 8649
				bind = node5
			}
			*/

	1. restart

			$ sudo service ganglia-monitor restart

1. grids

	1. create `grid` and update `data_source`

			$ sudo pico /etc/ganglia/gmetad.conf
			...
			# 1. gridname
			gridname "beijing"

			# 2. update `data_source`
			data_source "servers" localhost 1.1.1.2:8556
			data_source "databases" 1.2.1.1:8557 1.2.1.2:8557

	1. edit `gmond` config

			# update `name` value
			$ sudo pico /etc/ganglia/gmond.conf
			# ...
			cluster {
				name = "databases"
				owner = "unspecified"
				latlong = "unspecified"
				url = "unspecified"
			}
			# ... do not comment out `mcast_join`
			udp_send_channel {
				mcast_join = node5
				port = 8557
				ttl = 1
			}
			# ... do not comment out `mcast_join`
			udp_recv_channel {
				mcast_join = node5
				port = 8557
				bind = node5
			}
			# ...
			tcp_accept_channel {
				port = 8557
			}

	1. restart `ganglia-monitor` on `each node`

			$ sudo service ganglia-monitor restart

	1. restart `ganglia-monitor, gmetad and apache2` on `master node`

			$ sudo service ganglia-monitor restart && sudo service gmetad restart && sudo service apache2 restart
