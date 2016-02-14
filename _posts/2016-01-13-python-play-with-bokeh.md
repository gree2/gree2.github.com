---
layout: post
title: "python play with bokeh"
description: ""
category: [viz, python]
tags: [python, bokeh, install, sample data]
---
{% include JB/setup %}


### install

1. package install

            $ conda install bokeh

            $ pip install bokeh

1. sample data

    1. python prompt

            >>> import bokeh.sampledata
            >>> bokeh.sampledata.download()

    1. from bash or windows command prompt

            $ python -c "import bokeh.sampledata; bokeh.sampledata.download()"
            Creating ~/.bokeh directory
            Creating /Users/hqlgree2/.bokeh/data directory
            Using data directory: /Users/hqlgree2/.bokeh/data
            Downloading: CGM.csv (1589982 bytes)
               1589982 [100.00%]
            Downloading: US_Counties.zip (3182088 bytes)
               3182088 [100.00%]
            Unpacking: US_Counties.csv
            Downloading: us_cities.json (713565 bytes)
                713565 [100.00%]

### demos check my [notebooks](https://github.com/gree2/notebooks/tree/master/play.with.bokeh)
