---
layout: post
title: "git tools edit an incorrect commit message"
description: ""
category: [git]
tags: [commit, message]
---
{% include JB/setup %}

### solution

1. $ git commit --amend -m "your new message"
1. $ git push origin master -f

[see here](http://stackoverflow.com/questions/179123/edit-an-incorrect-commit-message-in-git)