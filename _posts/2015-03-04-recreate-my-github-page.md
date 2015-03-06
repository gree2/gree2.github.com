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

### step 1 create repo on github

Create a new repo named [gree2.github.io](https://github.com/gree2/gree2.github.com)

where `gree2` is my github username

### step 2 install Jekyll-Bootstrap

    git clone https://github.com/plusjade/jekyll-bootstrap.git gree2.github.com
    cd gree2.github.com
    git remote set-url origin https://github.com/gree2/gree2.github.com.git
    git push origin master

### step 3 visit my site

[http://gree2.github.io](http://gree2.github.io)

### step 4 create a post

    $ rake post title="recreate my github page"

Feel free to leave a comment.

