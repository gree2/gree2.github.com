---
layout: post
title: "pygraphviz on mac"
description: ""
category: [viz]
tags: [pygraphviz, graphviz, mac]
---
{% include JB/setup %}


### install

1. install graphviz

            $ brew install graphviz

1. download [pygraphviz](https://pypi.python.org/pypi/pygraphviz)

1. edit `setup.py`

            $ pico setup.py

            ...
            include_dirs=['/usr/local/lib/graphviz'],
            library_dirs=['/url/local/include/graphviz'],
            ....

1. [install](http://www.douban.com/note/535451318/?type=rec)

            $ sudo python setup.py install --include-path=/usr/local/Cellar/graphviz/2.38.0/include --library-path=/usr/local/Cellar/graphviz/2.38.0/lib

1. quick [example](http://pygraphviz.github.io/examples.html)

            >>> import pygraphviz as pgv
            >>> G=pgv.AGraph()
            >>> G.add_node('a')
            >>> G.add_edge('b','c')
            >>> G
            strict graph {
                    a;
                    b -- c;
            }

            # load dot file
            >>> G=pgv.AGraph("file.dot")

### reference

1. [quick-install](http://pygraphviz.github.io/documentation/development/install.html#quick-install)

1. [document](http://pygraphviz.github.io/documentation/pygraphviz-1.3rc1/)