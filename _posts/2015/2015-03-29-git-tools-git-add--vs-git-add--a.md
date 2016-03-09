---
layout: post
title: "git tools git add . vs git add -A"
description: ""
category: [git]
tags: [add ., add -A]
---
{% include JB/setup %}

### $ git status

    On branch master
    Your branch is up-to-date with 'origin/master'.

    Changes not staged for commit:
      (use "git add/rm <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    _posts/2015-03-09-getting-started-realm.md

    Untracked files:
      (use "git add <file>..." to include in what will be committed)

        _posts/2015-03-09-getting-started-realm-for-ios.md
        _posts/2015-03-29-getting-started-realm-for-android.md

    no changes added to commit (use "git add" and/or "git commit -a")

### $ git add .

    warning: You ran 'git add' with neither '-A (--all)' or '--ignore-removal',
    whose behaviour will change in Git 2.0 with respect to paths you removed.
    Paths like '_posts/2015-03-09-getting-started-realm.md' that are
    removed from your working tree are ignored with this version of Git.

    * 'git add --ignore-removal <pathspec>', which is the current default,
      ignores paths you removed from your working tree.

    * 'git add --all <pathspec>' will let you also record the removals.

    Run 'git status' to check the paths you removed from your working tree.

### $ git status

    On branch master
    Your branch is up-to-date with 'origin/master'.

    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        new file:   _posts/2015-03-09-getting-started-realm-for-ios.md
        new file:   _posts/2015-03-29-getting-started-realm-for-android.md

    Changes not staged for commit:
      (use "git add/rm <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    _posts/2015-03-09-getting-started-realm.md

### $ git add -A

    $ git status

    On branch master
    Your branch is up-to-date with 'origin/master'.

    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        renamed:    _posts/2015-03-09-getting-started-realm.md -> _posts/2015-03-09-getting-started-realm-for-ios.md
        new file:   _posts/2015-03-29-getting-started-realm-for-android.md