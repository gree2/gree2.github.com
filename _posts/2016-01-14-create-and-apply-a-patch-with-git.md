---
layout: post
title: "create and apply a patch with git"
description: ""
category: [git]
tags: [clone, checkout, log, apply, am, signoff, pretty]
---
{% include JB/setup %}


### [how to](https://ariejan.net/2009/10/26/how-to-create-and-apply-a-patch-with-git/)

1. create a patch

    1. clone and create a new branch

            $ git clone url_to_some_repo
            $ cd repo
            $ git checkout -b fix_sth

    1. hack whatever you want

    1. list you commits

            $ git log --pretty=oneline -3

    1. create a patch against master

            $ git format-patch master --stdout > fix_sth.patch

1. apply a patch

    1. what changes are in the patch

            $ git apply --stat fix_sth.patch

    1. test patch before actually apply it

            $ git apply --check fix_sth.patch

    1. `am` allows you to sign off an applied patch

            $ git am --signoff < fix_sth.patch
