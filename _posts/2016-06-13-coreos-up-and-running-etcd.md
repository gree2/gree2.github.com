---
layout: post
title: "coreos up and running etcd"
description: ""
category: [auto, linux]
tags: [coreos, etcd]
---
{% include JB/setup %}


### info

1. open source

1. distibuted key value store

1. using raft algorithm

### ops

1. reading and writing to etcd
	
	1. logging into the host

			$ cd coreos-vagrant
			$ vagrant up
			$ vagrant ssh

	1. from host machine

			# 1. set get
			$ etcdctl set /message1 book1
			$ etcdctl get /message1

			# 2. set get
			$ curl -L -X PUT http://127.0.0.1:2379/v2/keys/message2 -d value="book2"
			$ curl -L http://127.0.0.1:2379/v2/keys/message2

			# 3. delete
			$ etcdctl rm /message1
			$ curl -L -X DELETE http://127.0.0.1:2379/v2/keys/message2

			# 4. key value pair
			$ etcdctl set /foo-dict/foo-key somekey
			$ etcdctl ls  /foo-dict --recursive
			/foo-dict/foo-key
			
			$etcdctl get /foo-dict/foo-key
			somekey

	1. from application container

			# 1. check docker0's ip
			$ echo "$(ifconfig docker0 | awk '/\<inet\>/ { print $2 }'):2379"
			192.168.99.100:2379

			# 2. run bash shell
			$ docker run -it alpine ash

			# 3. install curl to alpine linux
			$ apk update && apk add curl
			$ curl -L http://192.168.99.100:2379/v2/keys

			# 4. exit
			$ ^+d

1. watching changes in etcd

	1. create a dir in etcd

			$ etcdctl mkdir /foo-data

	1. watch for changes in this directory

			$ etcdctl watch /foo-data --recursive

	1. open new terminal

			$ cd coreos-vagrant
			$ vagrant ssh

	1. add a new key to /foo-data directory

			$ etcdctl set /foo-data/book is_cool

	1. in first terminal should see a notification

			is_cool

1. ttl

	1. set a ttl to 30 sec

			$ etcdctl set /foo "i'm expiring in 30 sec" --ttl 30

	1. verify that the key is still there

			$ etcdctl get /foo
			i'm expiring in 30 sec

	1. check again after 30 sec

			$ etcdctl get /foo
			Error: 100: Key not found (/foo) [17053]
