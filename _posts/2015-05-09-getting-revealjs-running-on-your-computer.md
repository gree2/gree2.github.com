---
layout: post
title: "getting reveal.js running on your computer"
description: ""
category: [tool]
tags: [presentation, setup, reveal.js]
---
{% include JB/setup %}


### installation

1. basic setup

    1. download the [latest version of reveal.js](https://github.com/hakimel/reveal.js/releases)

    1. unzip and replace `index.html` with your own

    1. open `index.html` in a browser to view it

1. full setup

    1. install [node.js](http://nodejs.org/) download [node-v0.12.2.pkg](http://nodejs.org/dist/v0.12.2/node-v0.12.2.pkg)

    1. install [grunt](http://gruntjs.com/getting-started#installing-the-cli)

            $ sudo npm update -g npm

            $ sudo npm install -g grunt-cli
            Password:
            /usr/local/bin/grunt -> /usr/local/lib/node_modules/grunt-cli/bin/grunt
            grunt-cli@0.1.13 /usr/local/lib/node_modules/grunt-cli
            ├── resolve@0.3.1
            ├── nopt@1.0.10 (abbrev@1.0.5)
            └── findup-sync@0.1.3 (lodash@2.4.2, glob@3.2.11)

    1. clone `reveal.js` repo

            $ git clone https://github.com/hakimel/reveal.js.git
            Cloning into 'reveal.js'...
            remote: Counting objects: 7619, done.
            remote: Compressing objects: 100% (4/4), done.
            remote: Total 7619 (delta 3), reused 2 (delta 2), pack-reused 7612
            Receiving objects: 100% (7619/7619), 5.53 MiB | 17.00 KiB/s, done.
            Resolving deltas: 100% (4016/4016), done.
            Checking connectivity... done.

    1. navigate to `reveal.js` folder

            $ cd reveal.js

    1. install dependencies

            $ npm install

            > node-sass@0.9.6 install /Users/hqlgree2/Documents/github/reveal.js/node_modules/node-sass
            > node build.js

            child_process: customFds option is deprecated, use stdio instead.

    1. serve the presentation and monitor source files for changes

            $ grunt serve

    1. open [http://localhost:8000](http://localhost:8000) to view your presentation

            # you can change port by using
            $ grunt serve --port 8080

### demo

* [check out the live demo](http://lab.hakim.se/reveal-js/)

* [example-presentations](https://github.com/hakimel/reveal.js/wiki/Example-Presentations)