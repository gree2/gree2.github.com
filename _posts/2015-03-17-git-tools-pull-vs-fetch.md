---
layout: post
title: "git tools pull vs fetch"
description: ""
category: [git]
tags: [pull, fetch]
---
{% include JB/setup %}

### git pull or git fetch

don't use `git pull`, use `git fetch` and then `git merge`

$ git pull origin master
# will pull changes from the `origin` remote `master` branch
# and merge them to the local checked-out branch

$ git fetch origin
# get changes from the remote repo called `origin`
# into my local repo
# git fetch doesn't touch your working tree
# to actually bring the changes from the remote branch
# into your working tree, you have to do a `git merge`

$ git checkout master
# so i'm working on `master` then i can
# merge in the changes that we've just got from origin
$ git merge origin/master

[see here](http://stackoverflow.com/questions/2883840/differences-between-git-pull-origin-master-git-pull-origin-master)