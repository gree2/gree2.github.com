---
layout: post
title: "pydata2015 berlin bokeh taylor server"
description: ""
category: [viz, python]
tags: [bokeh, server, line]
---
{% include JB/setup %}


### code

1. [taylor_server](https://github.com/chdoig/pydata2015-berlin-bokeh/blob/gh-pages/examples/Server/taylor_server.py)

    1. original

            from __future__ import print_function

            import numpy as np
            import sympy as sy

            from bokeh.browserlib import view
            from bokeh.document import Document
            from bokeh.models.glyphs import Line
            from bokeh.models import Plot, DataRange1d, LinearAxis, ColumnDataSource, Grid, Legend
            from bokeh.session import Session
            from bokeh.models.widgets import Slider, TextInput, HBox, VBox, Dialog

            document = Document()
            session = Session()
            session.use_doc('taylor_server')
            session.load_document(document)

            xs = sy.Symbol('x')
            expr = sy.exp(-xs)*sy.sin(xs)
            order = 1

            def taylor(fx, xs, order, x_range=(0, 1), n=200):
                x0, x1 = x_range
                x = np.linspace(float(x0), float(x1), n)

                fy = sy.lambdify(xs, fx, modules=['numpy'])(x)
                tx = fx.series(xs, n=order).removeO()

                if tx.is_Number:
                    ty = np.zeros_like(x)
                    ty.fill(float(tx))
                else:
                    ty = sy.lambdify(xs, tx, modules=['numpy'])(x)

                return x, fy, ty

            def update_data():
                x, fy, ty = taylor(expr, xs, order, (-2*sy.pi, 2*sy.pi), 200)

                plot.title = "%s vs. taylor(%s, n=%d)" % (expr, expr, order)
                legend.legends = [
                    ("%s"         % expr, [line_f_glyph]),
                    ("taylor(%s)" % expr, [line_t_glyph]),
                ]
                source.data = dict(x=x, fy=fy, ty=ty)
                slider.value = order

                session.store_document(document)

            source = ColumnDataSource(data=dict(x=[], fy=[], ty=[]))

            xdr = DataRange1d()
            ydr = DataRange1d()

            plot = Plot(x_range=xdr, y_range=ydr, plot_width=800, plot_height=400)

            line_f = Line(x="x", y="fy", line_color="blue", line_width=2)
            line_f_glyph = plot.add_glyph(source, line_f)
            plot.add_layout(line_f_glyph)

            line_t = Line(x="x", y="ty", line_color="red", line_width=2)
            line_t_glyph = plot.add_glyph(source, line_t)
            plot.add_layout(line_t_glyph)

            xaxis = LinearAxis()
            plot.add_layout(xaxis, 'below')

            yaxis = LinearAxis()
            plot.add_layout(yaxis, 'left')

            xgrid = Grid(dimension=0, ticker=xaxis.ticker)
            ygrid = Grid(dimension=1, ticker=yaxis.ticker)

            legend = Legend(orientation="bottom_left")
            plot.add_layout(legend)

            def on_slider_value_change(obj, attr, old, new):
                global order
                order = int(new)
                update_data()

            def on_text_value_change(obj, attr, old, new):
                try:
                    global expr
                    expr = sy.sympify(new, dict(x=xs))
                except (sy.SympifyError, TypeError, ValueError) as exception:
                    dialog.content = str(exception)
                    dialog.visible = True
                    session.store_objects(dialog)
                else:
                    update_data()

            dialog = Dialog(title="Invalid expression")

            slider = Slider(start=1, end=20, value=order, step=1, title="Order:")
            slider.on_change('value', on_slider_value_change)

            text = TextInput(value=str(expr), title="Expression:")
            text.on_change('value', on_text_value_change)

            inputs = HBox(children=[slider, text])
            layout = VBox(children=[inputs, plot, dialog])

            document.add(layout)
            update_data()

            if __name__ == "__main__":
                link = session.object_link(document.context)
                print("Please visit %s to see the plots" % link)
                view(link)
                print("\npress ctrl-C to exit")
                session.poll_document(document)

    1. convert to bokeh 0.11

            import numpy as np
            import sympy as sy

            from bokeh.io import curdoc
            from bokeh.client import push_session
            from bokeh.models import Plot, DataRange1d, LinearAxis, ColumnDataSource, Grid, Legend
            from bokeh.models.glyphs import Line
            from bokeh.models.widgets import Slider, TextInput, HBox, VBox, Dialog

            document = curdoc()
            session = push_session(document)

            xs = sy.Symbol('x')
            expr = sy.exp(-xs)*sy.sin(xs)
            order = 1


            def taylor(fx, xs, order, x_range=(0, 1), n=200):
                x0, x1 = x_range
                x = np.linspace(float(x0), float(x1), n)

                fy = sy.lambdify(xs, fx, modules=['numpy'])(x)
                tx = fx.series(xs, n=order).removeO()

                if tx.is_Number:
                    ty = np.zeros_like(x)
                    ty.fill(float(tx))
                else:
                    ty = sy.lambdify(xs, tx, modules=['numpy'])(x)

                return x, fy, ty


            def update_data():
                x, fy, ty = taylor(expr, xs, order, (-2*sy.pi, 2*sy.pi), 200)

                plot.title = "%s vs. taylor(%s, n=%d)" % (expr, expr, order)
                legend.legends = [
                    ("%s" % expr, [line_f_glyph]),
                    ("taylor(%s)" % expr, [line_t_glyph]),
                ]
                source.data = dict(x=x, fy=fy, ty=ty)
                slider.value = order

            source = ColumnDataSource(data=dict(x=[], fy=[], ty=[]))

            xdr = DataRange1d()
            ydr = DataRange1d()

            plot = Plot(x_range=xdr, y_range=ydr, plot_width=800, plot_height=400)

            line_f = Line(x="x", y="fy", line_color="blue", line_width=2)
            line_f_glyph = plot.add_glyph(source, line_f)
            plot.add_layout(line_f_glyph)

            line_t = Line(x="x", y="ty", line_color="red", line_width=2)
            line_t_glyph = plot.add_glyph(source, line_t)
            plot.add_layout(line_t_glyph)

            xaxis = LinearAxis()
            plot.add_layout(xaxis, 'below')

            yaxis = LinearAxis()
            plot.add_layout(yaxis, 'left')

            xgrid = Grid(dimension=0, ticker=xaxis.ticker)
            ygrid = Grid(dimension=1, ticker=yaxis.ticker)

            legend = Legend(location="bottom_left")
            plot.add_layout(legend)


            def on_slider_value_change(attr, old, new):
                global order
                order = int(new)
                update_data()


            def on_text_value_change(attr, old, new):
                try:
                    global expr
                    expr = sy.sympify(new, dict(x=xs))
                except (sy.SympifyError, TypeError, ValueError) as exception:
                    dialog.content = str(exception)
                    dialog.visible = True
                else:
                    update_data()

            dialog = Dialog(title="Invalid expression")

            slider = Slider(start=1, end=20, value=order, step=1, title="Order:")
            slider.on_change('value', on_slider_value_change)

            text = TextInput(value=str(expr), title="Expression:")
            text.on_change('value', on_text_value_change)

            inputs = HBox(children=[slider, text])
            layout = VBox(children=[inputs, plot, dialog])

            document.add_root(layout)
            update_data()

            if __name__ == "__main__":
                # open the doc in browser
                session.show()
                # run forever
                session.loop_until_closed()

### [class LinearAxis(**kwargs)](http://bokeh.pydata.org/en/latest/docs/reference/models/axes.html#bokeh.models.axes.LinearAxis)

1. base bokeh.models.axes.ContinuousAxis

1. description

    1. an axis that picks nice numbers for tick locations on a linear scale

    1. configured with a BasicTickFormatter by default

### [class Grid(**kwargs)](http://bokeh.pydata.org/en/latest/docs/reference/models/grids.html#bokeh.models.grids.Grid)

1. base bokeh.models.renderers.GuideRenderer

1. description

    1. display horizontal or vertical grid lines at locations given by a supplied Ticker

1. attr

    1. `dimension` which dimension the axis grid lines will intersect

            the x-axis is dimension 0 (vertical grid lines)
            the y-axis is dimension 1 (horizontal grid lines)

    1. `ticker` the ticker to use for computing locations for the grid lines
