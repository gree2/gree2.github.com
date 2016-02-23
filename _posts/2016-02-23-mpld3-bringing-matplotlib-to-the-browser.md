---
layout: post
title: "mpld3 bringing matplotlib to the browser"
description: ""
category: [python, viz]
tags: [matplotlib, d3.js, notebook, stackoverflow]
---
{% include JB/setup %}


### info

1. bringing together

    1. [matplotlib](http://www.matplotlib.org/) popular python-based graphing library

    1. [d3.js](http://d3js.org/) popular js library

1. simple api for explorting matplotlib to html code

### examples

1. [example gallery](http://mpld3.github.io/examples/index.html#example-gallery)

    1. [defining a custom plugin](http://mpld3.github.io/examples/custom_plugin.html)

    1. [draggable points example](http://mpld3.github.io/examples/drag_points.html)

    1. [html tooltip plugin](http://mpld3.github.io/examples/html_tooltips.html)

    1. [image with mouse position](http://mpld3.github.io/examples/mouse_position.html)

    1. [linked brushing example](http://mpld3.github.io/examples/linked_brush.html)

    1. [mpld3 logo idea](http://mpld3.github.io/examples/mpld3_logo.html)

    1. [patches and paths](http://mpld3.github.io/examples/heart_path.html)

    1. [scatter plot with tooltips](http://mpld3.github.io/examples/scatter_tooltip.html)

    1. [visualizing random walks](http://mpld3.github.io/examples/random_walk.html)

1. [notebook example](http://mpld3.github.io/notebooks/index.html#notebook-examples)

    1. [plugins in mpld3](http://mpld3.github.io/notebooks/custom_plugins.html)

    1. [demo of mpld3](http://mpld3.github.io/notebooks/mpld3_demo.html)

### stackoverflow

1. [python how to add tooltips to pandas plots](http://stackoverflow.com/questions/30418868/python-how-to-add-tooltips-to-pandas-plots)

            import mpld3, pandas as pd
            mpld3.enable_notebook()

            df = pd.DataFrame(np.cumsum(np.random.normal(0,1,(5,1000)),axis=1).T)
            axes = df.plot(figsize=(14,4), colormap='spectral');

            labels = list(df.columns.values)
            for i in range(len(labels)):
                tooltip = mpld3.plugins.LineLabelTooltip(axes.get_lines()[i], labels[i])
                mpld3.plugins.connect(plt.gcf(), tooltip) 