---
layout: post
title: "play with prometheus docs"
description: ""
category: [monitor]
tags: [prometheus, zsh, bundle]
---
{% include JB/setup %}


### steps

1. prerequisites

    1. ruby and gems

            $ git clone https://github.com/gree2/docs.git
            $ cd docs
            $ bundle
            zsh: /usr/local/bin/bundle: bad interpreter: /System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/bin: no such file or directory

    1. fixed [problem-with-bundle](https://stackoverflow.com/questions/3958092/problem-with-bundle)

            $ gem uninstall bundler
            $ gem install bundler

    1. install gems

            $ bundle
            Fetching gem metadata from https://rubygems.org/.........
            Fetching rack 1.5.2
            Installing rack 1.5.2
            Fetching adsf 1.2.0
            Installing adsf 1.2.0
            Fetching builder 3.2.2
            Installing builder 3.2.2
            Using bundler 1.16.2
            ...
            Fetching rspec 3.5.0
            Installing rspec 3.5.0
            Bundle complete! 14 Gemfile dependencies, 43 gems now installed.
            Use `bundle info [gemname]` to see where a bundled gem is installed.

1. building

        # gen static site
        $ make build
        # stored in output dir
        ...
              create  [0.22s]  output/blog/index.html
              create  [0.02s]  output/blog/feed.xml

        Site compiled in 179.12s.

1. dev server

        # rebuild the site whenever relevant files change
        $ bundle exec guard

        # start the local dev server
        $ bundle exec nanoc view
        # http://localhost:3000/
