---
layout: post
title: "up and running medis"
description: ""
category: [setup]
tags: [redis, medis, npm, electron]
---
{% include JB/setup %}


### steps

1. download medis source

    1. from [github](https://github.com/luin/medis/releases)

1. running locally

    1. install deps

            $ npm install

    1. compile assets

            $ num run build
            ...
            npm ERR! missing script: build
            ...

    1. run with electron

            $ npm run electron

### fixed

1. npm ERR! missing script: build

    1. [how to solve npm run build dev missing script](https://stackoverflow.com/questions/41071819/how-to-solvenpm-run-build-dev-missing-script)

            npm run build

            will check `script` section of the `package.json`
            try to find a script titled `build`

            if no script named `build`
            it will error out

    1. check package.json

            $ more package.json
            ...
            "scripts": {
                "start": "rm Medis.app/Contents/Resources/app.asar; NODE_ENV=debug Medis.app/Contents/MacOS/Electron .",
                "dev": "./bin/dev",
                "deploy": "NODE_ENV=production ./bin/deploy",
                "release": "NODE_ENV=production ./bin/release",
                "sign": "./bin/sign",
                "unsign": "./bin/unsign",
                "tomedis": "./bin/tomedis"
            },
            ...

            # no build found
            # so use release
            $ npm run release
            ...
            npm ERR! Failed at the medis@0.6.1 release script 'NODE_ENV=production ./bin/release'.
            Make sure you have the latest version of node.js and npm installed
            ...

1. [upgrade nodejs to the latest version on mac os](https://stackoverflow.com/questions/11284634/upgrade-nodejs-to-the-latest-version-on-mac-os)

    1. use `brew` to update node

            $ brew update
            $ brew upgrade node
            $ npm install -g npm

    1. use `npm`

            $ sudo npm cache clean -f
            $ sudo npm install -g n
            $ sudo n stable
