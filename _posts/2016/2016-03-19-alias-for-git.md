---
layout: post
title: "alias for git"
description: ""
category: [command, git]
tags: [command, git, alias, add, status, push, pull, origin, master]
---
{% include JB/setup %}


### methods

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
            Changes not staged for commit:
              (use "git add <file>..." to update what will be committed)
              (use "git checkout -- <file>..." to discard changes in working directory)

                    modified:   _posts/2016-03-19-alias-for-git.md



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
