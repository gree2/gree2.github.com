---
layout: post
title: "git tools add vs reset"
description: ""
category: [git]
tags: [git, add, reset]
---
{% include JB/setup %}

1. created two new post

            $ git status
            On branch master
            Your branch is up-to-date with 'origin/master'.

            Untracked files:
              (use "git add <file>..." to include in what will be committed)

                    _posts/2015-03-23-android-emulator-shotcuts.md
                    _posts/2015-03-23-batch-net-use-xcopy-and-mkdir.md

            nothing added to commit but untracked files present (use "git add" to track)

1. add to staged area

            $ git add .

            $ git status
            On branch master
            Your branch is up-to-date with 'origin/master'.

            Changes to be committed:
              (use "git reset HEAD <file>..." to unstage)

                    new file:   _posts/2015-03-23-android-emulator-shotcuts.md
                    new file:   _posts/2015-03-23-batch-net-use-xcopy-and-mkdir.md

1. then found file name has a typo `shotcuts`

    * fix this typo use `git reset HEAD <file>`

            $ git reset HEAD _posts/2015-03-23-android-emulator-shotcuts.md

    * check git status again

            $ git status
            On branch master
            Your branch is up-to-date with 'origin/master'.

            Changes to be committed:
              (use "git reset HEAD <file>..." to unstage)

                    new file:   _posts/2015-03-23-batch-net-use-xcopy-and-mkdir.md

            Untracked files:
              (use "git add <file>..." to include in what will be committed)

                    _posts/2015-03-23-android-emulator-shortcuts.md

1. add and show status

            $ git add .

            $ git status
            On branch master
            Your branch is up-to-date with 'origin/master'.

            Changes to be committed:
              (use "git reset HEAD <file>..." to unstage)

                    new file:   _posts/2015-03-23-android-emulator-shortcuts.md
                    new file:   _posts/2015-03-23-batch-net-use-xcopy-and-mkdir.md

1. commit these changes

            $ git commit -m "emulator shortcuts and batch net xcopy files"
            [master 1b6dd42] emulator shortcuts and batch net xcopy files
             2 files changed, 134 insertions(+)
             create mode 100644 _posts/2015-03-23-android-emulator-shortcuts.md
             create mode 100644 _posts/2015-03-23-batch-net-use-xcopy-and-mkdir.md
