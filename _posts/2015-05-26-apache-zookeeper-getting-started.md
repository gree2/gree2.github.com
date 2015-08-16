---
layout: post
title: "apache zookeeper getting started"
description: ""
category: [bigdata]
tags: [zookeeper, leader, follower, observer]
---
{% include JB/setup %}


### info

1. centralize servie

	* maintaining configuration infomation

	* naming

	* providing distributed synchronization

	* providing group servies

1. znode container for

	* data

	* other nodes

1. [download](http://www.apache.org/dyn/closer.cgi/zookeeper/)

1. [reference](https://www.youtube.com/watch?v=2jzpkdYHwSQ)

### setup a 3 node cluster

1. setup and config

	1. on host1

			$ sudo mv zookeeper-3.5.0-alpha/ /usr/local/zookeeper

			$ mkdir /usr/local/zookeeper/data
			$ chown -R host1 /usr/local/zookeeper

			$ export PATH=$PATH:/usr/local/zookeeper/bin

			$ pico /usr/local/zookeeper/conf/zoo.cfg
			tickTime=2000
			initLimit=10
			syncLimit=5
			dataDir=/usr/local/zookeeper/data
			clientPort=2181
			server.1=host1:20010:20011
			server.2=host2:20010:20011
			server.3=host2:20010:20011

			$ pico /usr/local/zookeeper/data/myid
			1

	1. on host2

			$ sudo mv zookeeper-3.5.0-alpha/ /usr/local/zookeeper

			$ mkdir /usr/local/zookeeper/data
			$ chown -R host1 /usr/local/zookeeper

			$ export PATH=$PATH:/usr/local/zookeeper/bin

			$ pico /usr/local/zookeeper/conf/zoo.cfg
			tickTime=2000
			initLimit=10
			syncLimit=5
			dataDir=/usr/local/zookeeper/data
			clientPort=2181
			server.1=host1:20010:20011
			server.2=host2:20010:20011
			server.3=host2:20010:20011

			$ pico /usr/local/zookeeper/data/myid
			2

	1. on host3

			$ sudo mv zookeeper-3.5.0-alpha/ /usr/local/zookeeper

			$ mkdir /usr/local/zookeeper/data
			$ chown -R host1 /usr/local/zookeeper

			$ export PATH=$PATH:/usr/local/zookeeper/bin

			$ pico /usr/local/zookeeper/conf/zoo.cfg
			tickTime=2000
			initLimit=10
			syncLimit=5
			dataDir=/usr/local/zookeeper/data
			clientPort=2181
			server.1=host1:20010:20011
			server.2=host2:20010:20011
			server.3=host2:20010:20011

			$ pico /usr/local/zookeeper/data/myid
			3

1. start

	1. on host1

			$ zkServer.sh start

	1. on host2

			$ zkServer.sh start

	1. on host3

			$ zkServer.sh start

1. status

	1. on host1

			$ zkServer.sh status

	1. on host2

			$ zkServer.sh status

	1. on host3

			$ zkServer.sh status

1. client demo

			$ zkCli.sh -server host1:2181
			[zk:...] ls
			[zk:...] create /my_app /somedata
			[zk:...] ls
			[zk:...] get /my_app
			[zk:...] ls
			[zk:...] set /my_app /newdata
			[zk:...] ls
			[zk:...] delete /my_app
			[zk:...] ls