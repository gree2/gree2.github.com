---
layout: post
title: "running a bokeh server"
description: ""
category: [viz, python]
tags: [bokeh, server, serve, json, push, periodic, timeout, asynchronous, callback, nginx]
---
{% include JB/setup %}


### purpose

1. architecture

            +---------+                          
            | python  |  plots ranges axes glyphs
            +---------+                          
                ↑ ↓           +--------------+   
                ↑ ↓  json <== | bokeh server |   
                ↑ ↓           +--------------+   
            +---------+                          
            | BokehJS |                          
            +---------+                          

1. sync `model objects` in python and in the browser with each other

    1. ui and tool events => computations or queries => python

    1. auto push updates => ui

    1. periodic, timeout, asynchronous callbacks drive streaming updates

### use case scenarios

1. local or individual use

1. creating deployable apps

1. shared publishing

### specifying `output_server`

1. check my post [2016-01-29-deploying-the-bokeh-server]({% post_url 2016-01-29-deploying-the-bokeh-server %})

### connecting with `bokeh.client`

1. introduced new `tornado` and `websocket-based` server in bokeh 0.11

1. client api for interacting with bokeh server

    1. trigger updates to the plots and widgets in the browser

    1. periodic or asynchronous callbacks

1. demo

    1. first step is to start boekh server

            $ bokeh serve

    1. code

            import numpy as np
            form numpy import pi

            from bokeh.client import push_session
            from bokeh.driving import cosine
            from bokeh.plotting import figure, curdoc

            x = np.linspace(0, 4 * pi, 80)
            y = np.sin(x)

            p = figure()
            r1 = p.line([0, 4 * pi], [-1, 1], color='firebrick')
            r2 = p.line(x, y, color='navy', line_width=4)
            session = push_session(curdoc())

            # periodic callback
            @cosine(w=0.03)
            def update(step):
                r2.data_source.data['y'] = y * step
                r2.glyph.line_alpha = 1 - 0.8 * abs(step)

            # run every 50 milliseconds
            curdoc().add_periodic_callback(update, 50)

            # open the doc in browser
            session.show()

            # run forever
            session.loop_until_closed()

### building bokeh apps

1. single module format

    1. demo

            # myapp.py

            import numpy as np

            from bokeh.models import Button
            from bokeh.palettes import RdYlBu3
            from bokeh.plotting import figure, curdoc, vplot

            # create a plot and style its properties
            p = figure(x_range(0, 100), y_range(0, 100), toolbar_location=None)
            p.border_fill_color = 'black'
            p.background_file_color = 'black'
            p.outline_line_color = None
            p.grid.grid_line_color = None

            # add a text renderer to out plot (no data yet)
            r = p.text(x=[], y=[], text=[], text_color=[], text_font_size='20pt',
                       text_baseline='middle', text_align='center'
            )

            i = 0

            # callback that will add a number in a rondom location
            def callback():
                global i
                ds.data['x'].append(np.random.random() * 70 + 15)
                ds.data['y'].append(np.random.random() * 70 + 15)
                ds.data['text_color'].append(RdYlBu3[i % 3])
                ds.data['text'].append(str(i))
                ds.trigger('data', ds.data, ds.data)
                i = i + 1

            # add button widget and config callback
            button = Button(label='press me')
            button.on_click(callback)

            # put button and plot in a layout and add to doc
            curdoc().add_root(vplot(button, p))

    1. run

            $ bokeh serve --show myapp.py

1. directory format

    1. run

            $ bokeh serve --show myapp

    1. minimum

            myapp
                +--- main.py

    1. full set of files

            myapp
                +---main.py
                +---server_lifecycle.py
                +---theme.yaml

    1. include data files and models

            myapp
                +---data
                |       +---things.csv
                +---main.py
                +---models
                |       +---custom.js
                +---server_lifecycle.py
                +---theme.yaml

    1. demo

            from os.path import dirname, join
            import pandas

            pandas.read_csv(join(dirname(__file__), 'data', 'things.csv'))

1. callbacks and events

    1. js callbacks in the browser

    1. py callbacks with jupyter interactors

    1. py callbacks in bokeh apps

    1. py callbacks with `bokeh.client`

1. application theming

1. lifecycle hooks

    1. server

            def on_server_loaded(server_context):
                pass

            def on_server_unloaded(server_context):
                pass

    1. session

            def on_session_created(session_context):
                pass

            def on_session_destroyed(session_context):
                pass

### example and video tutorials

### integration with web app frameworks

### deployment scenarios

1. standalone bokeh server

1. reverse proxying with nginx

1. load balancing with nginx

1. process control with supervisord

1. a full [e.g.](http://demo.bokehplots.com/) with automation

    1. [saltstack](http://saltstack.com/)

    1. [demo.bokehplots.com](https://github.com/bokeh/demo.bokehplots.com)