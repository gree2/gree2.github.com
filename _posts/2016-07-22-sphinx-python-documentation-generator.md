---
layout: post
title: "sphinx python documentation generator"
description: ""
category: [python]
tags: [sphinx, python, documentation]
---
{% include JB/setup %}


### [first steps with sphinx](http://www.sphinx-doc.org/en/stable/tutorial.html)

1. install sphinx

        $ pip install sphinx

1. setup documentation sources

        $ shpinx-quickstart

1. difine document structure

    1. toctree directive initially is empty

            ..  toctree::
                :maxdepth: 2

    1. add `intro` and `tutorial`

            ..  toctree:
                :maxdepth: 2

                intro
                tutorial

    1. create `intro.rst` and `tutorial.rst` in `source` folder

            $ touch intro.rst tutorial.rst

1. running the build

    1. use `sphinx-build

            $ sphinx-build -b html sourcedir builddir

    1. use make with `Makefile` or `make.bat`

            $ make html

### [read the docs sphinx theme](https://github.com/snide/sphinx_rtd_theme)

1. installation via package

    1. install

            $ pip install sphinx_rtd_theme

    1. config

            # edit `conf.py` file

            import sphinx_rtd_theme
            html_theme = "sphinx_rtd_theme"
            html_theme_path = [sphinx_rtd_theme.get_html_theme_path()]

### reference

1. [www.sphinx-doc.org](http://www.sphinx-doc.org/en/stable/#)

1. [sphinx_rtd_theme](https://github.com/snide/sphinx_rtd_theme)

1. [networkx documentation](http://networkx.readthedocs.io/en/latest/)

1. [readthedocs.org](https://readthedocs.org/)
