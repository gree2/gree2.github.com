---
layout: post
title: "git sync between repos"
description: ""
category: [git]
tags: [pull, push]
---
{% include JB/setup %}


### steps

1. create 2 git repos and clone to local

            $ git clone https://github.com/gree2/bar.git
            $ git clone https://github.com/gree2/foo.git

1. check repos

    1. bar

            $ cd bar

            $ git st
            On branch master

            Initial commit

            nothing to commit (create/copy files and use "git add" to track)

            $ ls

            $ git log
            fatal: your current branch 'master' does not have any commits yet

    1. foo

            $ cd foo

            $ git st
            On branch master

            Initial commit

            nothing to commit (create/copy files and use "git add" to track)

            $ ls

            $ git log
            fatal: your current branch 'master' does not have any commits yet

1. do some change in `bar` repo

    1. add python code `hello.py`

            #! coding: utf-8

            hello('hello')

    1. commit it and push

            $ git add .

            $ git status
            On branch master

            Initial commit

            Changes to be committed:
              (use "git rm --cached <file>..." to unstage)

                    new file:   hello.py


            $ git commit -m "add hello.py"
            [master (root-commit) 7de004d] add hello.py
             1 file changed, 3 insertions(+)
             create mode 100644 hello.py

            $ git push origin master
            Counting objects: 3, done.
            Writing objects: 100% (3/3), 241 bytes | 0 bytes/s, done.
            Total 3 (delta 0), reused 0 (delta 0)
            To https://github.com/gree2/bar.git
             * [new branch]      master -> master

1. let's now change to `foo` repo

    1. check its remote

            $ git remote -v
            origin  https://github.com/gree2/foo.git (fetch)
            origin  https://github.com/gree2/foo.git (push)

    1. add a remote `upstream` point to `bar` repo

            $ git remote add upstream https://github.com/gree2/bar.git

            $ git remote -v
            origin  https://github.com/gree2/foo.git (fetch)
            origin  https://github.com/gree2/foo.git (push)
            upstream        https://github.com/gree2/bar.git (fetch)
            upstream        https://github.com/gree2/bar.git (push)

    1. pull from `upstream`

            $ git pull upstream master
            remote: Counting objects: 3, done.
            remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
            Unpacking objects: 100% (3/3), done.
            From https://github.com/gree2/bar
             * branch            master     -> FETCH_HEAD
             * [new branch]      master     -> upstream/master

            $ git branch -a
            * master
              remotes/upstream/master

            $ ls
            hello.py

            $ git status
            On branch master
            Your branch is based on 'origin/master', but the upstream is gone.
              (use "git branch --unset-upstream" to fixup)
            nothing to commit, working directory clean

    1. push to `origin`

            $ git push origin master
            Counting objects: 3, done.
            Writing objects: 100% (3/3), 241 bytes | 0 bytes/s, done.
            Total 3 (delta 0), reused 0 (delta 0)
            To https://github.com/gree2/foo.git
             * [new branch]      master -> master

1. repeat again

    1. in repo `bar`

            $ git status
            On branch master
            Your branch is up-to-date with 'origin/master'.
            Changes not staged for commit:
              (use "git add <file>..." to update what will be committed)
              (use "git checkout -- <file>..." to discard changes in working directory)

                    modified:   hello.py

            no changes added to commit (use "git add" and/or "git commit -a")

            $ git add .

            $ git status
            On branch master
            Your branch is up-to-date with 'origin/master'.
            Changes to be committed:
              (use "git reset HEAD <file>..." to unstage)

                    modified:   hello.py


            $ git commit -m "add doc string to hello.py"
            [master 6aa43c3] add doc string to hello.py
             1 file changed, 1 insertion(+)

            $ git push origin master
            Counting objects: 3, done.
            Delta compression using up to 8 threads.
            Compressing objects: 100% (2/2), done.
            Writing objects: 100% (3/3), 298 bytes | 0 bytes/s, done.
            Total 3 (delta 0), reused 0 (delta 0)
            To https://github.com/gree2/bar.git
               7de004d..6aa43c3  master -> master

    1. in repo `foo`

            $ git pull upstream master
            remote: Counting objects: 3, done.
            remote: Compressing objects: 100% (2/2), done.
            remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
            Unpacking objects: 100% (3/3), done.
            From https://github.com/gree2/bar
             * branch            master     -> FETCH_HEAD
               7de004d..6aa43c3  master     -> upstream/master
            Updating 7de004d..6aa43c3
            Fast-forward
             hello.py | 1 +
             1 file changed, 1 insertion(+)

            $ git status
            On branch master
            Your branch is ahead of 'origin/master' by 1 commit.
              (use "git push" to publish your local commits)
            nothing to commit, working directory clean

            $ git push origin master
            Counting objects: 3, done.
            Delta compression using up to 8 threads.
            Compressing objects: 100% (2/2), done.
            Writing objects: 100% (3/3), 298 bytes | 0 bytes/s, done.
            Total 3 (delta 0), reused 0 (delta 0)
            To https://github.com/gree2/foo.git
               7de004d..6aa43c3  master -> master

### tbc