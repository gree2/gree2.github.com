---
layout: post
title: "deploying the bokeh server"
description: ""
category: [viz, python]
tags: [bokeh, bokeh-server, conda, downgrade, streaming]
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

1. steps

    1. write code `demo.py`

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

    1. start `bokeh-server`

            $ bokeh-server

            Bokeh Server Configuration
            ==========================
            python version : 2.7.11
            bokeh version  : 0.10.0
            listening      : 127.0.0.1:5006
            backend        : memory
            python options : debug:OFF, verbose:OFF, filter-logs:OFF, multi-user:OFF
            js options     : debugjs:OFF

    1. run demo.py

            $ python demo.py
            Using saved session configuration for http://localhost:5006/
            To override, pass 'load_from_config=False' to Session

    1. from `bokeh-server` side you will see

            2016-01-30 18:13:27,521:INFO:tornado.access:200 GET /bokeh/userinfo/ (127.0.0.1) 1.52ms
            2016-01-30 18:13:27,547:INFO:tornado.access:200 POST /bokeh/doc/ (127.0.0.1) 2.18ms
            2016-01-30 18:13:27,550:INFO:tornado.access:200 GET /bokeh/getdocapikey/797be60d-03f7-4558-a60f-6b6b904d5960 (127.0.0.1) 1.12ms
            2016-01-30 18:13:27,570:INFO:tornado.access:200 POST /bokeh/bb/797be60d-03f7-4558-a60f-6b6b904d5960/gc (127.0.0.1) 17.87ms
            2016-01-30 18:13:27,574:INFO:tornado.access:200 GET /bokeh/bb/797be60d-03f7-4558-a60f-6b6b904d5960/ (127.0.0.1) 1.37ms
            2016-01-30 18:13:27,630:INFO:tornado.access:200 POST /bokeh/bb/797be60d-03f7-4558-a60f-6b6b904d5960/bulkupsert (127.0.0.1) 26.75ms
            2016-01-30 18:13:27,739:INFO:tornado.access:200 POST /bokeh/bb/797be60d-03f7-4558-a60f-6b6b904d5960/bulkupsert (127.0.0.1) 18.40ms
            2016-01-30 18:13:27,991:INFO:tornado.access:200 GET /bokeh/doc/797be60d-03f7-4558-a60f-6b6b904d5960/c4c6deb9-2c17-4370-93bf-9e1a8a81e9f9 (127.0.0.1) 17.51ms
            2016-01-30 18:13:28,263:INFO:tornado.access:200 POST /bokeh/bb/797be60d-03f7-4558-a60f-6b6b904d5960/bulkupsert (127.0.0.1) 15.45ms
            2016-01-30 18:13:28,441:INFO:tornado.access:200 GET /bokeh/wsurl/ (127.0.0.1) 0.97ms
            2016-01-30 18:13:28,489:INFO:tornado.access:200 GET /bokeh/objinfo/797be60d-03f7-4558-a60f-6b6b904d5960/c4c6deb9-2c17-4370-93bf-9e1a8a81e9f9 (127.0.0.1) 18.74ms
            2016-01-30 18:13:28,784:INFO:tornado.access:200 POST /bokeh/bb/797be60d-03f7-4558-a60f-6b6b904d5960/bulkupsert (127.0.0.1) 14.50ms
            2016-01-30 18:13:29,304:INFO:tornado.access:200 POST /bokeh/bb/797be60d-03f7-4558-a60f-6b6b904d5960/bulkupsert (127.0.0.1) 14.42ms
            2016-01-30 18:13:29,826:INFO:tornado.access:200 POST /bokeh/bb/797be60d-03f7-4558-a60f-6b6b904d5960/bulkupsert (127.0.0.1) 14.21ms
            ...

### fixed

1. but if your bokeh's version is higher than `0.10.0`

    1. you can't run `bokeh-server`

            $ bokeh-server

            The 'bokeh-server' command from versions 0.10 and earlier no longer exists.
            Please see the documentation for the new 'bokeh' command here:

            http://bokeh.pydata.org/en/latest/docs/user_guide/cli.html

    1. [you can't](https://github.com/vitruvianscience/OpenDeep/issues/23) import `cursession` from `bokeh.plotting`

            # so you must downgrade your bokeh
            $ conda install bokeh=0.10.0

1. you can run `bokeh` with bokeh's version higher than 0.10.0

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
