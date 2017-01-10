---
layout: post
title: "pydata2015 berlin bokeh random walk"
description: ""
category: [viz, python]
tags: [bokeh, server, line, curdoc, session, document, callback]
---
{% include JB/setup %}


### code

1. [random_walk](https://github.com/chdoig/pydata2015-berlin-bokeh/blob/gh-pages/examples/Server/random_walk.ipynb)

    1. original

            import collections
            import datetime
            import time

            import numpy as np

            from bokeh.plotting import cursession, figure, show, output_notebook
            # To run these examples you must execute the command
            # python bokeh-server in the top-level Bokeh source directory first.

            output_notebook(url="default")

            TS_MULT_us = 1e6
            UNIX_EPOCH = datetime.datetime(1970, 1, 1, 0, 0) #offset-naive datetime

            def int2dt(ts, ts_mult=TS_MULT_us):
                """Convert timestamp (integer) to datetime"""
                return(datetime.datetime.utcfromtimestamp(float(ts)/ts_mult))
                    
            def td2int(td, ts_mult=TS_MULT_us):
                """Convert timedelta to integer"""
                return(int(td.total_seconds()*ts_mult))
                    
            def dt2int(dt, ts_mult=TS_MULT_us):
                """Convert datetime to integer"""
                delta = dt - UNIX_EPOCH
                return(int(delta.total_seconds()*ts_mult))
                    
            def int_from_last_sample(dt, td):
                return(dt2int(dt) - dt2int(dt) % td2int(td))

            TS_MULT = 1e3
            td_delay = datetime.timedelta(seconds=0.5)
            delay_s = td_delay.total_seconds()
            delay_int = td2int(td_delay, TS_MULT)

            value = 1000 # initial value
            N = 100 # number of elements into circular buffer

            buff = collections.deque([value]*N, maxlen=N)

            t_now = datetime.datetime.utcnow()
            ts_now = dt2int(t_now, TS_MULT)
            t = collections.deque(np.arange(ts_now-N*delay_int, ts_now, delay_int), maxlen=N)

            p = figure(x_axis_type="datetime")
            p.line(list(t), list(buff), color="#0000FF", name="line_example")

            renderer = p.select(dict(name="line_example"))[0]
            ds = renderer.data_source
            show(p)
            while True:
                ts_now = dt2int(datetime.datetime.utcnow(), 1e3)
                t.append(ts_now)
                ds.data['x'] = list(t)

                value += np.random.uniform(-1, 1)
                buff.append(value)
                ds.data['y'] = list(buff)
                
                cursession().store_objects(ds)
                time.sleep(delay_s)

    1. convert to bokeh 0.11

            #! coding: utf-8

            import collections
            import datetime
            import time

            import numpy as np

            from bokeh.io import curdoc
            from bokeh.client import push_session
            from bokeh.plotting import figure, show


            # *To run these examples you must execute the command
            # `bokeh serve` in the top-level Bokeh source directory first.*

            TS_MULT_us = 1e6
            # offset-naive datetime
            UNIX_EPOCH = datetime.datetime(1970, 1, 1, 0, 0)


            def int2dt(ts, ts_mult=TS_MULT_us):
                """Convert timestamp (integer) to datetime"""
                return(datetime.datetime.utcfromtimestamp(float(ts)/ts_mult))


            def td2int(td, ts_mult=TS_MULT_us):
                """Convert timedelta to integer"""
                return(int(td.total_seconds()*ts_mult))


            def dt2int(dt, ts_mult=TS_MULT_us):
                """Convert datetime to integer"""
                delta = dt - UNIX_EPOCH
                return(int(delta.total_seconds()*ts_mult))


            def int_from_last_sample(dt, td):
                return(dt2int(dt) - dt2int(dt) % td2int(td))


            TS_MULT = 1e3
            td_delay = datetime.timedelta(seconds=0.5)
            delay_s = td_delay.total_seconds()
            delay_int = td2int(td_delay, TS_MULT)

            # initial value
            value = 1000
            # number of elements into circular buffer
            N = 100

            buff = collections.deque([value]*N, maxlen=N)


            t_now = datetime.datetime.utcnow()
            ts_now = dt2int(t_now, TS_MULT)
            t = collections.deque(np.arange(ts_now-N*delay_int, ts_now, delay_int), maxlen=N)

            p = figure(x_axis_type="datetime")
            p.line(list(t), list(buff), color="#0000FF", name="line_example")

            renderer = p.select(dict(name="line_example"))[0]
            ds = renderer.data_source


            def update():
                ts_now = dt2int(datetime.datetime.utcnow(), 1e3)
                t.append(ts_now)
                ds.data['x'] = list(t)

                global value
                value += np.random.uniform(-1, 1)
                buff.append(value)
                ds.data['y'] = list(buff)

            # run every 50 milliseconds
            curdoc().add_periodic_callback(update, delay_s * 100)

            session = push_session(curdoc())
            # open the doc in browser
            session.show()

            # run forever
            session.loop_until_closed()

### [curdoc()](http://bokeh.pydata.org/en/latest/docs/reference/io.html#bokeh.io.curdoc)

1. bokeh.io.curdoc

1. description

    1. return the document for the current default state

### [add_periodic_callback(callback, period_milliseconds)](http://bokeh.pydata.org/en/latest/docs/reference/document.html#bokeh.document.Document.add_periodic_callback)

1. bokeh.document.Document.add_periodic_callback

1. description

    1. add a callback to be invoked on a session periodic

1. parameters

    1. `callback` callable the callback function to execute

    1. `period_milliseconds` int the number of milliseconds that should be between each callback execution

            note:
            periodic callbacks only work within the context of a bokeh server session
            this function will no effect when bokeh outputs to standalone html or jupyter notebook cells

### [push_session(document, session_id=None, url='default', app_path='/', io_loop=None)](http://bokeh.pydata.org/en/latest/docs/reference/client.html#bokeh.client.session.push_session)

1. bokeh.client.session.push_session

1. description

    1. create a session by pushing the given document to the server

    1. overwriting any existing server side document

    1. `session.document` in the returned session will be your supplied document

            while the connection to the server if open
            changes make on server side will be applied to this document
            changes made on client side will be synced to the server

1. parameters

    1. `document` bokeh.document.Document the document to be pushed and set as session.document

    1. `session_id` str, optional the name of the session, None to autogenerate a random one (default: None)

    1. `url` str, optional the base server url to connect to (default: None)

    1. `app_path` str, optional relative path to the app on the server (default: '/')

    1. `io_loop` tornado.ioloop.IOLoop, optional the IOLoop to used for the websocket
