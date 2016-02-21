---
layout: post
title: "animation with ipythonblocks"
description: ""
category: [python]
tags: [ipython, ipythonblocks, animation]
---
{% include JB/setup %}


### [animation](https://github.com/jiffyclub/ipythonblocks)

1. using `clear_output` function

	1. demo import

			from ipythonblocks import BlockGrid
			import time
			from IPython.display import clear_output

	1. demo `time.sleep()`

			grid = BlockGrid(3, 3)
			previous_block = None

			for block in grid:
				clear_output()
				block.green = 255

				if previous_block:
					previous_block.green = 0

				grid.show()

				previous_block = block
				time.sleep(0.2)

	1. demo `BlockGrid.animate()`

			grid = BlockGrid(3, 3)
			previous_block = None

			for block in grid.animate():
				block.green = 255

				if previous_block:
					previous_block.green = 0

				previous_block = block

	1. demo `BlockGrid.flash()`

			grid = BlockGrid(3, 3)

			previous_block = None

			indices = [[(0, 0), (0, 2), (2, 0), (2, 2)], [(0, 1), (1, 0), (1, 2), (2, 1)]] * 10

			for ind in indices:
			    for i in ind:
			        grid[i[0], i[1]].green = 255
			    
			    grid.flash(display_time=0.1)
			    
			    for i in ind:
			        grid[i[0], i[1]].green = 0

			grid.show()
