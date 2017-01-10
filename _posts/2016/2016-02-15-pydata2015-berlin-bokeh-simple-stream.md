---
layout: post
title: "pydata2015 berlin bokeh simple stream"
description: ""
category: [viz, python]
tags: [bokeh, server, line]
---
{% include JB/setup %}


### code

1. [simple_stream](https://github.com/chdoig/pydata2015-berlin-bokeh/blob/gh-pages/examples/Server/simple_stream.py)

    1. original

            import time

            import numpy as np

            from bokeh.plotting import cursession, figure, show, output_server
            from bokeh.models import GlyphRenderer

            x = np.linspace(0, 4*np.pi, 200)
            y = np.sin(x)

            output_server("simple_stream")

            p = figure(title="Simple streaming example")
            p.line(x, y, color="#2222aa", line_width=2)

            show(p)

            ds = p.select({"type": GlyphRenderer})[0].data_source
            while True:
                oldx = ds.data["x"]
                newx = np.hstack([oldx, [oldx[-1] + 4*np.pi/200]])
                ds.data["x"] = newx
                ds.data["y"] = np.sin(newx)
                cursession().store_objects(ds)
                time.sleep(0.5)

    1. convert to bokeh 0.11

            #! conding utf-8

            import time
            import numpy as np

            from bokeh.io import curdoc
            from bokeh.client import push_session
            from bokeh.models import GlyphRenderer
            from bokeh.plotting import figure, show

            x = np.linspace(0, 4*np.pi, 200)
            y = np.sin(x)

            p = figure(title="Simple streaming example")
            p.line(x, y, color="#2222aa", line_width=2)
            ds = p.select({"type": GlyphRenderer})[0].data_source


            def update():
                oldx = ds.data["x"]
                newx = np.hstack([oldx, [oldx[-1] + 4*np.pi/200]])
                ds.data["x"] = newx
                ds.data["y"] = np.sin(newx)

            # run every 5 milliseconds
            curdoc().add_periodic_callback(update, 5)

            session = push_session(curdoc(), 'simple-stream')
            # open the doc in browser
            session.show()
            # run forever
            session.loop_until_closed()

### [class GlyphRenderer(**kwargs)](http://bokeh.pydata.org/en/latest/docs/reference/models/renderers.html#bokeh.models.renderers.GlyphRenderer)

1. base bokeh.models.renderers.DataRenderer

1. attr

    1. `data_source` local data source to use when rendering glyph on the plot

    1. `glyph` the glyph to render, in conjunction with supplied data source and ranges

    1. `hover_glyph` an optional glyph ofr inspected points

                e.g. those that are being hoverd over by a HoverTool

    1. `level` specifies the level in which to render the glyph

    1. `nonselection_glyph` an optional glyph used for explicitly non-selected points

    1. `selection_glyph` an optional glyph used for selected points

    1. `x_range_name` a particular(named) x-range to use for computing screen locations when rendering glyph on the plot

            if unset use the default x-range

    1. `y_range_name` see x_range_name
