---
layout: post
title: "python data analysis library pandas"
description: ""
category: [python]
tags: [analysis, library, pandas]
---
{% include JB/setup %}


### info

* [website](http://pandas.pydata.org/)

* open source, bsd-licensed library

* high-performace

* easy to use data structures and data analysis tools

### get pandas

1. dependencies

    1. [numpy](http://www.numpy.org/) 1.7.0 or higher

    1. [python-dateutil](http://labix.org/python-dateutil) 1.5 or higher

    1. [pytz](http://pytz.sourceforge.net/) for time zone support

1. recommended dependencies

    1. [numexpr](http://code.google.com/p/numexpr/) for accelerating certain numerical operations

    1. [bottlenect](http://berkeleyanalytics.com/bottleneck) for accelerating certain types of `nan` evaluations

                uses specialized cython routines to achieve large speedups

1. optional dependencies

    1. [cython](http://www.cython.org/) only necessary to build development version 0.19.1 or higher

    1. [scipy](http://www.scipy.org/) miscellaneous statistical functions

    1. [pytables](http://www.pytables.org/) necessary for `hdf5-based storage` 3.0.0 or higher

    1. [sqlaichemy](http://www.sqlalchemy.org/) for sql database support 0.8.1 or higher

    1. [matplotlib](http://matplotlib.sourceforge.net/) for plotting

    1. [statsmodels](http://statsmodels.sourceforge.net/) needed for parts of `pandas.stats`

    1. [openpyxl, xlrd/slwt](http://www.python-excel.org/) 1.6.1 < openpyxml < 2.0.0 needed for excel i/o

    1. [xlsxwriter](https://pypi.python.org/pypi/XlsxWriter) alternative excel writer

    1. [boto](https://pypi.python.org/pypi/boto) necessary for amazon s3 access

    1. one of [pyqt4](http://www.riverbankcomputing.com/software/pyqt/download), [pyside](http://qt-project.org/wiki/Category:LanguageBindings::PySide), [pygtk](http://www.pygtk.org/), [xsel](http://www.vergenet.net/~conrad/software/xsel/), or [xclip](http://sourceforge.net/projects/xclip/) necessary to use `read_clipboard()`

    1. google's [python-gflags](http://code.google.com/p/python-gflags/) and [google-api-python-client](http://github.com/google/google-api-python-client) needed for `gbq`

    1. [setuptools](https://pypi.python.org/pypi/setuptools/) needed for `gbq`

    1. [httplib2](http://pypi.python.org/pypi/httplib2) needed for `gbq`

    1. one of the following combinations of libraries in needed to use the top-level `read_html()` function

        * [beautifulsoup4](http://www.crummy.com/software/BeautifulSoup) and [html5lib](https://github.com/html5lib/html5lib-python)

        * [beautifulsoup4](http://www.crummy.com/software/BeautifulSoup) and [lxml](http://lxml.de/)

        * [beautifulsoup4](http://www.crummy.com/software/BeautifulSoup) and [html5lib](https://github.com/html5lib/html5lib-python) and [lxml](http://lxml.de/)

        * only [lxml](http://lxml.de/)

1. installation instructions

    1. trying out, no installation required [wakari](https://wakari.io/)

    1. installing pandas with [anaconda](http://docs.continuum.io/anaconda/)

    1. installing pandas with [miniconda](http://conda.pydata.org/miniconda.html)

    1. installing from [pypi](http://pypi.python.org/pypi/pandas)

    1. installing using linux package manager

    1. installing from source