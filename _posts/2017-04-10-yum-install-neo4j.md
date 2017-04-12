---
layout: post
title: "yum install neo4j"
description: ""
category: [setup, linux]
tags: [neo4j]
---
{% include JB/setup %}


### steps

1. [](http://yum.neo4j.org/stable/?_ga=1.16221429.2122739151.1491729992)

1. steps

	1. you'll want our key:

			$ cd /tmp
			$ wget http://debian.neo4j.org/neotechnology.gpg.key
			$ rpm --import neotechnology.gpg.key

	1. add yum repo to /etc/yum.repos.d/neo4j.repo:

			$ cat <<EOF>  /etc/yum.repos.d/neo4j.repo
			[neo4j]
			name=Neo4j Yum Repo
			baseurl=http://yum.neo4j.org/stable
			enabled=1
			gpgcheck=1
			EOF

	1. Finally, install a package

			$ yum install neo4j
