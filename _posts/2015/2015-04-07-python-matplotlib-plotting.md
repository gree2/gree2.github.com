---
layout: post
title: "python matplotlib plotting"
description: ""
category: [python]
tags: [matplotlib, plot, pylab]
---
{% include JB/setup %}

### introduction

* [Matplotlib](http://matplotlib.org/) is probable the single most used python package for 2d-graphics

* it provides both a very quick way to visualize data from python and publication-quality figures in many formats

* IPython and the pylab mode

    * [IPython](http://ipython.org/) is an enhanced interactive python shell that has lots of intersting features including named inputs and outputs, access to shell commands, improved debugging and many more

    * it is central to the scientific-computing workflow in python for its use in combination with Matplotlib

* pylab

    * pylab provides a procedural interface to the matplotlib object-oriented plotting library

    * it is modeled closely after Matlab

    * the majority of plotting commands in pylab have Matlab analogs with similar arguments

### simple plot

1. get the data for the sine and cosine functions

            import numpy as np
            X = np.linspace(-np.pi, np.pi, 256, endpoint=True)
            C, S = np.cos(X), np.sin(X)

    * X is now a numpy array with 256 values ranging from -pi to +pi (included)

    * C is the cosine (256 values)

    * S is the sine (256 values)

1. to run the example you can type them in an ipython interactive session

            $ ipython --pylab

1. you can also download each of the example and run

            $ python example.py

1. plotting with default settings

            import pylab as pl
            import numpy as np

            X = np.linspace(-np.pi, np.pi, 256, endpoint=True)
            C, S = np.cos(X), np.sin(X)

            pl.plot(X, C)
            pl.plot(X, S)

            pl.show()

1. instantiating defaults

            import pylab as pl
            import numpy as np

            # Create a figure of size 8x6 inches, 80 dots per inch
            pl.figure(figsize=(8, 6), dpi=80)

            # Create a new subplot from a grid of 1x1
            pl.subplot(1, 1, 1)

            X = np.linspace(-np.pi, np.pi, 256, endpoint=True)
            C, S = np.cos(X), np.sin(X)

            # Plot cosine with a blue continuous line of width 1 (pixels)
            pl.plot(X, C, color="blue", linewidth=1.0, linestyle="-")

            # Plot sine with a green continuous line of width 1 (pixels)
            pl.plot(X, S, color="green", linewidth=1.0, linestyle="-")

            # Set x limits
            pl.xlim(-4.0, 4.0)

            # Set x ticks
            pl.xticks(np.linspace(-4, 4, 9, endpoint=True))

            # Set y limits
            pl.ylim(-1.0, 1.0)

            # Set y ticks
            pl.yticks(np.linspace(-1, 1, 5, endpoint=True))

            # Save figure using 72 dots per inch
            # savefig("exercice_2.png", dpi=72)

            # Show result on screen
            pl.show()

1. changing colors and line widths

            pl.figure(figsize=(10, 6), dpi=80)
            pl.plot(X, C, color="blue", linewidth=2.5, linestyle="-")
            pl.plot(X, S, color="red",  linewidth=2.5, linestyle="-")

1. setting limits

            pl.xlim(X.min() * 1.1, X.max() * 1.1)
            pl.ylim(C.min() * 1.1, C.max() * 1.1)

1. setting ticks

            pl.xticks([-np.pi, -np.pi/2, 0, np.pi/2, np.pi])
            pl.yticks([-1, 0, +1])

1. setting tick labels

            pl.xticks([-np.pi, -np.pi/2, 0, np.pi/2, np.pi],
                      [r'$-\pi$', r'$-\pi/2$', r'$0$', r'$+\pi/2$', r'$+\pi$'])

            pl.yticks([-1, 0, +1],
                      [r'$-1$', r'$0$', r'$+1$'])

1. moving spines

            ax = pl.gca()  # gca stands for 'get current axis'
            ax.spines['right'].set_color('none')
            ax.spines['top'].set_color('none')
            ax.xaxis.set_ticks_position('bottom')
            ax.spines['bottom'].set_position(('data',0))
            ax.yaxis.set_ticks_position('left')
            ax.spines['left'].set_position(('data',0))

1. adding a legend

            pl.plot(X, C, color="blue", linewidth=2.5, linestyle="-", label="cosine")
            pl.plot(X, S, color="red",  linewidth=2.5, linestyle="-", label="sine")

            pl.legend(loc='upper left')

1. annotate some point

            t = 2 * np.pi / 3
            pl.plot([t, t], [0, np.cos(t)], color='blue', linewidth=2.5, linestyle="--")
            pl.scatter([t, ], [np.cos(t), ], 50, color='blue')

            pl.annotate(r'$sin(\frac{2\pi}{3})=\frac{\sqrt{3}}{2}$',
                        xy=(t, np.sin(t)), xycoords='data',
                        xytext=(+10, +30), textcoords='offset points', fontsize=16,
                        arrowprops=dict(arrowstyle="->", connectionstyle="arc3,rad=.2"))

            pl.plot([t, t],[0, np.sin(t)], color='red', linewidth=2.5, linestyle="--")
            pl.scatter([t, ],[np.sin(t), ], 50, color='red')

            pl.annotate(r'$cos(\frac{2\pi}{3})=-\frac{1}{2}$',
                        xy=(t, np.cos(t)), xycoords='data',
                        xytext=(-90, -50), textcoords='offset points', fontsize=16,
                        arrowprops=dict(arrowstyle="->", connectionstyle="arc3,rad=.2"))

1. devil is in the details

            for label in ax.get_xticklabels() + ax.get_yticklabels():
                label.set_fontsize(16)
                label.set_bbox(dict(facecolor='white', edgecolor='None', alpha=0.65))

### figures, subplots, axes and ticks

1. figures

    * a figure is the windows in the gui that has "Figure #" as title

    * `num` default `1` number of figure

    * `figsize` default `figure.figsize` figure size in inches (width, height)

    * `dpi` default `figure.dpi` resolution in dots per inch

    * `facecolor` default `figure.facecolor` color of the drawing background

    * `edgecolor` default `figure.edgecolor` color of edge around the drawing background

    * `frameon` default `True` draw figure frame or not

1. subplots

    * two

            +------------------+ 
            |                  |
            | subplot(2, 1, 1) |
            |                  |
            +------------------+ 
            +------------------+ 
            |                  |
            | subplot(2, 1, 2) |
            |                  |
            +------------------+ 

    * two

            +------------------+ +------------------+ 
            |                  | |                  |
            |                  | |                  |
            |                  | |                  |
            | subplot(1, 2, 1) | | subplot(1, 2, 2) |
            |                  | |                  |
            |                  | |                  |
            |                  | |                  |
            +------------------+ +------------------+ 

    * four

            +------------------+ +------------------+ 
            |                  | |                  |
            |                  | |                  |
            |                  | |                  |
            | subplot(2, 2, 1) | | subplot(2, 2, 2) |
            |                  | |                  |
            |                  | |                  |
            |                  | |                  |
            +------------------+ +------------------+ 
            +------------------+ +------------------+ 
            |                  | |                  |
            |                  | |                  |
            |                  | |                  |
            | subplot(2, 2, 3) | | subplot(2, 2, 4) |
            |                  | |                  |
            |                  | |                  |
            |                  | |                  |
            +------------------+ +------------------+ 

1. axes

            +---------------------------------------+ 
            |                                       |
            |                                       |
            |                                       |
            |          axes([0.1, 0.1, .8, .8])     |
            |                                       |
            |                                       |
            |                                       |
            |                                       |
            |    +------------------------+         |
            |    |                        |         |
            |    |                        |         |
            |    |axes([0.2, 0.2, .3, .3])|         |
            |    |                        |         |
            |    |                        |         |
            |    +------------------------+         |
            +---------------------------------------+ 

1. ticks

    * tick locators control the position of the ticks

            ax = pl.gca()
            ax.xaxis.set_major_locator(eval(locator))

    * there are several locator for different kind of requirements

            NullLocator()
            ============================================================

            MultipleLocator()
            =====|=====|=====|=====|=====|=====|=====|=====|=====|=====|
            0    1     2     3     4     5     6     7     8     9     10

            FixedLocator([0, 2, 8, 9, 10])
            ===========|===================================|=====|=====|
            0          2                                   8     9     10

            IndexLocator(3, 1)
            =====|=================|=================|=================|
                 1                 4                 7                 10

            LinearLocator(5)
            ==============|==============|==============|==============|
            .0           2.5            5.0            7.5           10.

            LogLocator(2, [1.0])
            =====|=====|===========|=======================|============
                 1     2           4                       8            

            AutoLocator()
            ===========|===========|===========|===========|===========|
            0          2           4           6           8           10

### other types of plots: examples and exercises

[examples and exercises]({% post_url 2015-04-10-python-matplotlib-plotting-examples-and-exercises %})

