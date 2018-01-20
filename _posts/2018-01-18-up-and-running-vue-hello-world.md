---
layout: post
title: "up and running vue hello world"
description: ""
category: [lang]
tags: [vue, cnpm]
---
{% include JB/setup %}


### steps

1. install webpack vue-cli

    1. check npm version

            $ npm -v
            5.6.0

    1. install use cnpm

            $ npm install -g cnpm --registry=https://registry.npm.taobao.org
            $ cnpm i webpack -g
            $ cnpm i vue-cli -g

1. create framework

    1. use vue-cli

            $ vue init webpack-simple hello
            ? Project name hello
            ? Project description a hello vue.js project
            ? Author hqlgree2 <hqlgree2@gmail.com>
            ? License MIT
            ? Use sass? Yes

               vue-cli · Generated "hello".

               To get started:

                 cd hello
                 npm install
                 npm run dev

    1. run hello

            $ cd hello
            $ ls
            README.md         index.html        package.json
            src               webpack.config.js

            # use cnpm instead of npm
            $ cnpm i
            ...
            npm notice created a lockfile as package-lock.json. You should commit this file.
            ...
            $ ls
            README.md         node_modules      package.json
            webpack.config.js index.html        package-lock.json src
            $ npm run dev

            $ cnpm install --save-dev sass-loader

1. choose ui

    1. desktop ui [bootstrap](http://v4-alpha.getbootstrap.com/getting-started/introduction/)

    1. mobile ui material ui [muse-ui](http://www.muse-ui.org/#/index)

    1. install

            $ cnpm install bootstrap@4.0.0-alpha.6 -S
            $ cnpm install font-awesome -S

    1. split section navbar sidebar content