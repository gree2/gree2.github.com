---
layout: post
title: "deploying the bokeh server"
description: ""
category: [vis]
tags: [bokeh, bokeh-server]
---
{% include JB/setup %}


### overview

1. provide

    1. publishing bokeh plots for wider audiences

    1. streaming data to automatically updating plots

    1. interactively visualizing very large datasets

    1. building dashboards and apps with sophisticated interactions

1. built on top of [flask](http://flask.pocoo.org/)

### concepts

1. core architecture of `bokeh-server`

    1. Document

    1. User

### running

1. if `bokeh` was installed by `setup.py` or `conda`

            $ bokeh-server

1. development mode

            $ python ./bokeh-server

### storing plots on the server

1. demo

            from bokeh.plotting import figure, output_server, show

            output_server('line')

            p = figure(plot_width=400, plot_height=400)

            # add a line renderer
            p.line([1, 2, 3, 4, 5], [6, 7, 2, 4, 5], line_width=2)

            show(p)

1. publishing to the server

    1. running in multi user mode

            output_server('myplot')

            # make some plots
            cursession().publish()

### streaming data with the server

1. demo

            import time
            from random import shuffle
            from bokeh.plotting import figure, output_server, cursession, show

            # prepare output to server
            output_server('animated_line')

            p = figure(plot_width=400, plot_height=400)
            p.line([1, 2, 3, 4, 5], [6, 7, 2, 4, 5], name='ex_line')
            show(p)

            # create some simple animation..
            # first get our figure example data source
            renderer = p.select(dict(name='ex_line'))
            ds = renderer[0].data_source

            while True:
                # update y data of the source object
                shuffle(ds.data['y'])

                # store the updated source on the server
                cursession().store_objects(ds)
                time.sleep(0.5)

### run bokeh-server

1. just run `bokeh-server`

            $ bokeh-server

            The 'bokeh-server' command from versions 0.10 and earlier no longer exists.
            Please see the documentation for the new 'bokeh' command here:

              http://bokeh.pydata.org/en/latest/docs/user_guide/cli.html

1. run `bokeh`

    1. bokeh -h

            $ bokeh -h
            usage: /Users/hqlgree2/anaconda/bin/bokeh [-h] [-v]
                                                      {html,json,secret,serve} ...

            positional arguments:
              {html,json,secret,serve}
                                    Sub-commands
                html                Create standalone HTML files for one or more
                                    applications
                json                Create JSON files for one or more applications
                secret              Create a Bokeh secret key for use with Bokeh server
                serve               Run a Bokeh server hosting one or more applications

            optional arguments:
              -h, --help            show this help message and exit
              -v, --version         show program's version number and exit

    1. bokeh serve

            $ bokeh serve
            DEBUG:bokeh.server.tornado:Allowed Host headers: ['localhost:5006']
            DEBUG:bokeh.server.tornado:These host origins can connect to the websocket: ['localhost:5006']
            DEBUG:bokeh.server.tornado:Patterns are: [('/?', <class 'bokeh.server.views.doc_handler.DocHandler'>, {'application_context': <bokeh.server.application_context.ApplicationContext object at 0x106a2f250>, 'bokeh_websocket_path': '/ws'}), ('/ws', <class 'bokeh.server.views.ws.WSHandler'>, {'application_context': <bokeh.server.application_context.ApplicationContext object at 0x106a2f250>, 'bokeh_websocket_path': '/ws'}), ('/autoload.js', <class 'bokeh.server.views.autoload_js_handler.AutoloadJsHandler'>, {'application_context': <bokeh.server.application_context.ApplicationContext object at 0x106a2f250>, 'bokeh_websocket_path': '/ws'}), ('/static/(.*)', <class 'bokeh.server.views.static_handler.StaticHandler'>)]
            INFO:bokeh.command.subcommands.serve:Starting Bokeh server on port 5006 with applications at paths ['/']
            DEBUG:bokeh.server.tornado:[pid 17900] 0 clients connected
