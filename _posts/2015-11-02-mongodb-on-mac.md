---
layout: post
title: "mongodb on mac"
description: ""
category: [bigdata]
tags: [mongodb, mac]
---
{% include JB/setup %}


### setup

1. with homebrew

	1. update homebrew's package database

			$ sudo brew update

	1. install mongodb

			# 1. install mongodb binaries
			$ brew install mongodb

			# 2. build mongodb from source with TLS/SSL support
			$ brew install mongodb --with-openssl

			# 3. install the latest development release
			$ brew install mongodb --devel

1. manually

	1. download

			$ curl -O https://fastdl.mongodb.org/osx/mongodb-osx-x86_64-3.0.7.tgz

	1. extract

			$ tar -zxf mongodb-osx*.tgz

	1. copy to target directory

			$ mkdir -p /usr/local/mongodb
			$ cp -R -n mongodb-osx-x86_64-3.0.7/ /usr/local/mongodb

	1. put binaries in the PATH variable

			$ export PATH=/usr/local/mongodb/bin:$PATH

### run mongodb

1. steps

	1. create the data directory

			# default the `mongod` process uses the `/data/db` directory
			$ mkdir -p /data/db

	1. set permissions

			# ensure that user account running `mongod`
			# has read & write permissions for the directory

	1. run mongodb

			$ mongod
			$ /usr/local/mongodb/bin/mongod
			$ mongod --dbpath /data/db

	1. stop mongodb

			$ ^+c
