---
layout: post
title: "centos cleanup root fs"
description: ""
category: [centos]
tags: [centos, du]
---
{% include JB/setup %}


### steps

1. sort disk usages

		du -ks /var/* | sort -nr | head

1. / info

		du --max-depth=1 -x -h /

### references

1. [how to cleanup root file system](https://www.centos.org/forums/viewtopic.php?t=15061)
