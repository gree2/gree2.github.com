---
layout: post
title: "ipythonblocks code plus color"
description: ""
category: [python]
tags: [ipython, ipythonblocks]
---
{% include JB/setup %}


### info

1. ipythonblocks and [ipythonblocks.org](http://ipythonblocks.org) are mode by [matt davis](http://penandpants.com/)

1. ipythonblocks can be found on [github](https://github.com/jiffyclub/ipythonblocks)

1. [ipythonblocks.org] is generously hosted by [rackspace](http://www.rackspace.com/)

### usage

1. [basic usage](http://nbviewer.jupyter.org/github/jiffyclub/ipythonblocks/blob/master/demos/ipythonblocks_demo.ipynb)

    1. grid

            from ipythonblocks import BlockGrid
            
            grid = BlockGrid(10, 10, fill=(123, 234, 123))
            grid

            gird[0, 0]

    1. iteration .row .col

            for block in grid:
                if block.row % 2 == 0 and block.col % 3 == 0:
                    block.red = 0
                    block.green = 0
                    block.blue = 0

            gird

    1. .height .width

            for r in range(grid.height):
                for c in range(grid.width):
                    sq = grid[r, c]
                    sq.red = 100

                    if r % 2 == 0:
                        sq.green = 15
                    else:
                        sq.green = 255

                    if c % 2 == 0:
                        sq.blue = 15
                    else:
                        sq.blue = 255

    1. show()

            grid.show()

            gird[5, 5].show()

    1. slicing

            sub_grid = grid[:, 5]
            sub_grid.show()

            for block in sub_grid:
                block.red = 255

            sub_grid

            gird

    1. set_colors()

            for block in grid[2:6, 2:4]:
                block.set_colors(245, 178, 34)

            grid

    1. copy()

            sub_copy = grid[3:-3, 3:-3].copy()
            sub_copy.show()

            sub_copy[:] = (0, 0, 0)
            sub_copy.show()

            grid

    1. modified by assigning rgb tuples

            gird[5] = (0, 0, 0)
            grid.show()

            grid[:, 5] = (255, 0, 0)
            grid.show()

            gird[-3:, -3:] = (0, 124, 124)
            grid.show()

            grid[1, 1] = (255, 255, 255)
            gird.show()

            gird[:, :] = (123, 234, 123)
            grid.show()

    1. block_size

            grid = BlockGrid(50, 50, block_size=5)
            grid

            # modified at any time
            grid.block_size = 2
            grid

    1. grid lines can be toggled by `lines_on`

            grid.lines_on = False
            grid

### demos

1. [Awesome Big Data Algorithms](https://www.youtube.com/watch?v=jKBwGlYb13w) 21:17

            from ipythonblocks import BlockGrid

            grid = BlockGrid(10, 10)
            x = grid.shape[0]
            y = grid.shape[1]

            for block in grid:
                r = block.row * 255 / float(x)
                g = block.col * 255 / float(y)
                block.red = r
                block.green = g

            grid