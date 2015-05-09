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

			$ npm update -g npm
			$ npm install -g grunt-cli

	1. clone `reveal.js` repo

			$ git clone https://github.com/hakimel/reveal.js.git

	1. navigate to `reveal.js` folder

			$ cd reveal.js

	1. install dependencies

			$ npm install

	1. serve the presentation and monitor source files for changes

			$ grunt serve

	1. open [http://localhost:8000](http://localhost:8000) to view your presentation

			# you can change port by using
			$ grunt serve --port 8080

### demo

* [check out the live demo](http://lab.hakim.se/reveal-js/)

* [example-presentations](https://github.com/hakimel/reveal.js/wiki/Example-Presentations)