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

### git pull

    $ git pull origin master

will `pull` changes from the `origin` remote `master` branch

and `merge` them to the local checked-out branch

### git fetch and merge

    $ git fetch origin

get changes from the remote repo called `origin` into my local repo

`git fetch` doesn't touch your working tree

to actually bring the changes from the remote branch

into your working tree you have to do a `git merge`

    $ git checkout master

so i'm working on `master` then i can

merge in the changes that we've just got from origin

    $ git merge origin/master

### eg

`$ git fetch`

    remote: Counting objects: 4, done.
    remote: Compressing objects: 100% (2/2), done.
    remote: Total 4 (delta 2), reused 4 (delta 2), pack-reused 0
    Unpacking objects: 100% (4/4), done.
    From https://github.com/gree2/gree2.github.com
       7651506..d1e8b2a  master     -> origin/master

`$ git merge origin/master`

    Updating 7651506..d1e8b2a
    Fast-forward
     _posts/2015-03-17-git-tools-pull-vs-fetch.md | 30 ++++++++++++++++++++++++++++
     1 file changed, 30 insertions(+)
     create mode 100644 _posts/2015-03-17-git-tools-pull-vs-fetch.md

[see here](http://stackoverflow.com/questions/2883840/differences-between-git-pull-origin-master-git-pull-origin-master)