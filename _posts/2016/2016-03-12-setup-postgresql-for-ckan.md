---
layout: post
title: "setup postgresql for ckan"
description: ""
category: [setup]
tags: [postgresql, ckan]
---
{% include JB/setup %}


### steps

1. list existing databases

			$ sudo -u postgres psql -l

			# check database encoding UTF8


1. create postgresql user `ckan_default`

			$ sudo -u postgres createuser -S -D -R -P ckan_default


1. create postgresql database

			$ sudo -u postgres createdb -O ckan_default ckan_default -E utf-8