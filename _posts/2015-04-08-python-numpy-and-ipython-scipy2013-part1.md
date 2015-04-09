---
layout: post
title: "python numpy and ipython scipy2013 part1"
description: ""
category: [python]
tags: [numpy, ipython, scipy2013]
---
{% include JB/setup %}


### materials

this tutorial is form [youtube](https://www.youtube.com/watch?v=UWmZAAfXds4)

* [github repo](http://git.io/bocNDg) 
* [zip files](http://git.io/G5i5qA)
* [anaconda](https://store.continuum.io)

### the scientists needs

* acquire data

    * simulation

    * experiment

* manipulate and process data

* visualize results

* communicate results

    * produce figures for reports or publications

    * write presentations

### why python

* easy to learn, easy to read, easy to maintain

* thriving ecosystem of scientific libraries

* vibrant community

* numpy and ipython

* commerical support

### the scientific python ecosystem

1. [Numpy](http://www.numpy.org/)

    1. NumPy is the fundamental package for scientific computing with Python. It contains among other things:

        * a powerful N-dimensional array object

        * sophisticated (broadcasting) functions

        * tools for integrating C/C++ and Fortran code

        * useful linear algebra, Fourier transform, and random number capabilities

    1. Besides its obvious scientific uses, NumPy can also be used as an efficient multi-dimensional container of generic data. Arbitrary data-types can be defined. This allows NumPy to seamlessly and speedily integrate with a wide variety of databases.

    1. Numpy is licensed under the BSD license, enabling reuse with few restrictions.

1. [IPython](http://ipython.org/)

    * IPython provides a rich architecture for interactive computing with:

        * Powerful interactive shells (terminal and [Qt-based](http://ipython.org/ipython-doc/stable/interactive/qtconsole.html)).

        * A browser-based [notebook](http://ipython.org/notebook.html) with support for code, text, mathematical expressions, inline plots and other rich media.

        * Support for interactive data visualization and use of [GUI toolkits](http://ipython.org/ipython-doc/stable/interactive/reference.html#gui-event-loop-support).

        * Flexible, [embeddable](http://ipython.org/ipython-doc/stable/interactive/reference.html#embedding-ipython) interpreters to load into your own projects.

        * Easy to use, high performance tools for [parallel computing](http://ipython.org/ipython-doc/stable/parallel/parallel_intro.html).


1. [Scipy](http://www.scipy.org/)

    1. SciPy (pronounced “Sigh Pie”) is a Python-based ecosystem of open-source software for mathematics, science, and engineering. In particular, these are some of the core packages:

        * `NumPy` base n-dimensional array package
        
        * `SciPy library` fundamental library for scientific computing
        
        * `Matplotlib` comprehensive 2d plotting
        
        * `IPython` enhanced interactive console
        
        * `Sympy` symbolic mathematics
        
        * `pandas` data structures & analysis
        

1. [Matplotlib](http://matplotlib.org/)

        matplotlib is a python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms. matplotlib can be used in python scripts, the python and ipython shell (ala MATLAB®* or Mathematica®†), web application servers, and six graphical user interface toolkits.

1. [Pandas](http://pandas.pydata.org/)

        pandas is an open source, BSD-licensed library providing high-performance, easy-to-use data structures and data analysis tools for the Python programming language.

1. [Sympy](http://www.sympy.org)

        SymPy is a Python library for symbolic mathematics. It aims to become a full-featured computer algebra system (CAS) while keeping the code as simple as possible in order to be comprehensible and easily extensible. SymPy is written entirely in Python and does not require any external libraries.

1. [Scikits-Learn](http://scikit-learn.org/)

    * Machine Learning in Python

        * Simple and efficient tools for data mining and data analysis

        * Accessible to everybody, and reusable in various contexts

        * Built on NumPy, SciPy, and matplotlib

        * Open source, commercially usable - BSD license


1. [PyTables](https://pytables.github.io/)

        PyTables is a package for managing hierarchical datasets and designed to efficiently and easily cope with extremely large amounts of data. You can download PyTables and use it for free. You can access documentation, some examples of use and presentations here.

1. [Cython](http://cython.org/)

        Cython is an optimising static compiler for both the Python programming language and the extended Cython programming language (based on Pyrex). It makes writing C extensions for Python as easy as Python itself.

### about this tutorial

* ipython

    * using the ipython notebook

    * help system, magic functions, aliases and history

* numpy

    * basic arrays, dtypes and numerical operations

    * indexing, slicing, reshaping and broadcasting

    * copies, views and fancy indexing

### about the materials

* ipython

    * an ipython notebook demonstrating the ipython notebook

    * a demo session of the ipython shell

* numpy

    * two ipython notebooks

    * (semi-)automatically converted from `python scientific lecture notes`

    * the generated HTML is include in the github repo / zip file and available online

### about ipython

* de facto python interpreter with bells and whistles

* since 2011: available in the browser as ipython notebook

        $ ipython notebook --pylab=inline

* the url to access the notebook will be printed

* numpy and matplotlib tools are available (pylab mode)

* plots will be displayed inside the notebook (inline mode)

### introduction to the ipython notebook

* the notebook consists of individual cells

* each cell may contain code or text (amongst other options)

* text format can be [markdown](http://daringfireball.net/projects/markdown), HTML or LaTeX

* code cell are run using

    * `shift+enter` will move to next cell

    * `ctrl+enter` will stay in the current cell

    * aways output the last line of code

* markdown cells can be modified by double-clicking on them

### help

* help on python objects using the `?`

        import IPython
        IPython?

* using the build-in help

        help(IPython)

        double click on the divider to close the help console

* tab completion is available

        IPython. # press tab after the period

### loading and saving


* Dashboard shows all notebooks in the directory where it was started from
* Use the dashboard to create new notebooks
* Files saved as JSON with the `.ipynb` extension
* Not meant for human consumption


* Use File -> Print View for a version without the controls
* Execute the cells who's output you want to show
* Save to HTML or print to PDF


* When running from someone else's server, use File -> Download
* Either as `*.ipynb` file
* Or as `*.py` file with the markdown as comments
* [`*.py` file can also be imported](http://ipython.org/ipython-doc/dev/interactive/htmlnotebook.html#exporting-a-notebook-and-importing-existing-scripts)


* There is also the online [nbviewer](http://nbviewer.ipython.org/)
* This can render a static version of a publicly available notebook.

### images and video

* images

        from IPython.display import Image

        Image(filename='images/logo.png')

        # Alternatively, load an image from a URL
        Image(url='http://octodex.github.com/images/pythocat.png')

* video

        from IPython.display import YouTubeVideo

        YouTubeVideo('F4rFuIb1Ie4')

### things i haven't covered

* Hiding output
* Debugging by attaching a QTConsole
* Restarting the kernel in case of crashes
* Cython magic

* [In-depth IPython tutorial this afternoon](https://conference.scipy.org/scipy2013/tutorial_detail.php?id=104)

### command line interaction

    $ anaconda_on
    $ ipython

    $ touch myfile.py
    $ echo "hello world"
    $ cat myfile.py
    $ pico myfile.py
    print ("hello world")

    $ python myfile.py
    $ ipython
    in [1]: run myfile.py
    hello world

    in [2]: hist

    in [3]: %cd
    in [4]: cd

    in [5]: %cpaste
    pasting code; enther '--' alone on the line to stop or user Ctrl-D
    s = "hello world"
    print(s)
    --

    in [6]: %debug
    in [7]: %quickref
    in [8]: %magic
    in [9]: alias

    in [10]: x = 10
    in [11]: x. # press tab here

    in [12]: clear
    in [13]: %reset
    Once deleted, variables cannot be recovered. Proceed (y/[n])?

    in [14]: x = 10
    in [15]: whos
