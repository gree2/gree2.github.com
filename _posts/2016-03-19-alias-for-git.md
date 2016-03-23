---
layout: post
title: "alias for git"
description: ""
category: [command, git]
tags: [command, git, alias, add, status, push, pull, origin, master]
---
{% include JB/setup %}


### 

1. use `.bash_profile`

    1. create

            # git
            alias gst="git status"
            alias gad="git add ."
            alias gaa="git add -A"
            function __gcm() {
                git commit -m "$*"
            }
            alias gcm=__gcm
            alias gps="git push origin master"
            alias gpl="git pull origin master"

    1. usage

            $ gst
            On branch master
            Your branch is up-to-date with 'origin/master'.
            nothing to commit, working directory clean


1. git's alias

    1. create

            $ git config --global alias.st status
            $ git config --global alias.pl pull
            $ git config --global alias.ps push

    1. list

            git config --list | grep alias
            alias.st=status
            alias.pl=pull origin master
            alias.ps=push origin master
            alias.aa=add -A
            alias.ad=add .
