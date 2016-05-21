---
layout: post
title: "react native day one"
description: ""
category: [react]
tags: [nodejs, n]
---
{% include JB/setup %}


### setup steps

1. install nodejs

    1. download form [nodejs.org](http://nodejs.org)

    1. or update

            $ npm install -g n
            $ n latest

1. install homebrew

    1. [install homebrew]({% post_url 2015-05-27-homebrew-the-missing-package-manager-for-os-x %})

            $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

    1. install watchman

            $ brew install watchman

    1. install flow

            $ brew install flow

1. install react-native-cli

            $ npm install -g react-native-cli


### fixed

1. unable to install homebrew

    1. [fixed](https://github.com/Homebrew/legacy-homebrew/issues/15138)

            $ sudo chown -R gree2 /usr/local/Library

    1. can't write to /usr/local/Cellar

            $ sudo chown -R gree2 /usr/local/Cellar

    1. it's just not linked

            $ brew link --overwrite watchman

1. missing write access to /usr/local/lib/node_modules

            $ sudo chown -R gree2 /usr/local/lib/