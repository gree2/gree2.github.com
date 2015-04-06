---
layout: post
title: "python run minecraft on mac"
description: ""
category: [python]
tags: [pyglet]
---
{% include JB/setup %}

### info

1. [best written projects on python github](http://www.reddit.com/r/Python/comments/1ls7vq/best_written_projects_on_python_github/)

1. [python minecraft](https://github.com/fogleman/Minecraft)

1. [c craft](https://github.com/fogleman/Craft)

### turn this project into a package

	import mc
	world = mc.World(...)
	world.set_block(x, y, z, mc.DIRT)
	mc.run(world)

### how to run

	pip install pyglet
	git clone https://github.com/fogleman/Minecraft.git
	cd Minecraft
	python main.py

### how to play

	w: forward
	s: back
	a: strafe left
	d: strafe right
	mouse: look around
	space: jump
	tab: toggle flying mode

### building

* selecting type of block to create

	* 1: brick
	* 2: grass
	* 3: sand

* mouse left-click: remove block

* mouse right-click: create block

### quitting

* esc: release mouse, then close window