---
layout: post
title: "recreate my github page"
description: ""
category: [jekyll, github]
tags: [jekyll, github]
---
{% include JB/setup %}

### history

In about 2012 I create my github page.

Today I delete that repo and recreate it.

Pay a visit to this [site](http://jekyllbootstrap.com/usage/jekyll-quick-start.html) to have a quick start.

### steps on mac

1. create repo on github

    1. create a new repo named [gree2.github.io](https://github.com/gree2/gree2.github.com)

    1. where `gree2` is my github username

1. install Jekyll-Bootstrap

            $ git clone https://github.com/plusjade/jekyll-bootstrap.git gree2.github.com
            $ cd gree2.github.com
            $ git remote set-url origin https://github.com/gree2/gree2.github.com.git
            $ git push origin master


1. create a post

    1. use rake

            $ cd gree2.github.com.git
            $ rake post title="recreate my github page"

    1. do some edit

    1. serve it

            $ jekyll serve

    1. browser it

            http://localhost:4000

    1. commit and push

            $ git add .
            $ git commit -m "recreate my github page"
            $ git push origin master

1. visit my site [http://gree2.github.io](http://gree2.github.io)

### setup jekyll on windows

1. [reference](http://yizeng.me/2013/05/10/setup-jekyll-on-windows/)
