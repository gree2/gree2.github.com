---
layout: post
title: "bokeh examples pydata2015 berlin bokeh"
description: ""
category: [viz, python]
tags: [bokeh, server, line]
---
{% include JB/setup %}


### code

1. [random_walk](https://github.com/chdoig/pydata2015-berlin-bokeh/blob/gh-pages/examples/Server/random_walk.ipynb)

    1. original

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

            from bokeh.client import push_session
            from bokeh.plotting import curdoc, figure, show


            # *To run these examples you must execute the command
            # `python bokeh-server` in the top-level Bokeh source directory first.*

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

            import time
            import numpy as np

            from bokeh.client import push_session
            from bokeh.models import GlyphRenderer
            from bokeh.plotting import curdoc, figure, show, output_server

            x = np.linspace(0, 4*np.pi, 200)
            y = np.sin(x)

            output_server("simple_stream")

            p = figure(title="Simple streaming example")
            p.line(x, y, color="#2222aa", line_width=2)
            ds = p.select({"type": GlyphRenderer})[0].data_source


            def update():
                oldx = ds.data["x"]
                newx = np.hstack([oldx, [oldx[-1] + 4*np.pi/200]])
                ds.data["x"] = newx
                ds.data["y"] = np.sin(newx)

            # run every 50 milliseconds
            curdoc().add_periodic_callback(update, 5)

            session = push_session(curdoc())
            # open the doc in browser
            session.show()
            # run forever
            session.loop_until_closed()

1. [linked_tap_server](https://github.com/chdoig/pydata2015-berlin-bokeh/blob/gh-pages/examples/Server/linked_tap_server.py)

    1. original

            from __future__ import print_function

            import numpy as np

            from bokeh.models import ColumnDataSource, DataRange1d, Plot, Circle, VBox, HBox, Button, TapTool
            from bokeh.document import Document
            from bokeh.session import Session
            from bokeh.browserlib import view

            document = Document()
            session = Session()
            session.use_doc('linked_tap_server')
            session.load_document(document)

            N = 9

            x = np.linspace(-2, 2, N)
            y = x**2

            source1 = ColumnDataSource(dict(x = x, y = y, size = [20]*N))
            xdr1 = DataRange1d()
            ydr1 = DataRange1d()
            plot1 = Plot(title="Plot1", x_range=xdr1, y_range=ydr1, plot_width=400, plot_height=400)
            plot1.tools.append(TapTool(plot=plot1))
            plot1.add_glyph(source1, Circle(x="x", y="y", size="size", fill_color="red"))

            source2 = ColumnDataSource(dict(x = x, y = y, color = ["blue"]*N))
            xdr2 = DataRange1d()
            ydr2 = DataRange1d()
            plot2 = Plot(title="Plot2", x_range=xdr2, y_range=ydr2, plot_width=400, plot_height=400)
            plot2.tools.append(TapTool(plot=plot2))
            plot2.add_glyph(source2, Circle(x="x", y="y", size=20, fill_color="color"))

            def on_selection_change1(obj, attr, _, inds):
                color = ["blue"]*N
                if inds['1d']['indices']:
                    [index] = inds['1d']['indices']
                    color[index] = "red"
                source2.data["color"] = color
                session.store_objects(source2)

            source1.on_change('selected', on_selection_change1)

            def on_selection_change2(obj, attr, _, inds):
                inds = inds['1d']['indices']
                if inds:
                    [index] = inds
                    size = [10]*N
                    size[index] = 40
                else:
                    size = [20]*N
                source1.data["size"] = size
                session.store_objects(source1)

            source2.on_change('selected', on_selection_change2)

            reset = Button(label="Reset")

            def on_reset_click():
                source1.selected = {
                    '0d': {'flag': False, 'indices': []},
                    '1d': {'indices': []},
                    '2d': {'indices': []}
                }
                source2.selected = {
                    '0d': {'flag': False, 'indices': []},
                    '1d': {'indices': []},
                    '2d': {'indices': []}
                }
                session.store_objects(source1, source2)

            reset.on_click(on_reset_click)

            vbox = VBox(children=[reset], width=150)
            hbox = HBox(children=[vbox, plot1, plot2])

            document.add(hbox)
            session.store_document(document)

            if __name__ == "__main__":
                link = session.object_link(document.context)
                print("Please visit %s to see the plots" % link)
                view(link)
                print("\npress ctrl-C to exit")
                session.poll_document(document)

    1. convert to bokeh 0.11

            import numpy as np

            from bokeh.client import push_session
            from bokeh.models import ColumnDataSource, DataRange1d, Plot, Circle, VBox, HBox, Button, TapTool
            from bokeh.plotting import curdoc

            document = curdoc()
            session = push_session(document)

            N = 9

            x = np.linspace(-2, 2, N)
            y = x**2
            source1 = ColumnDataSource(dict(x=x, y=y, size=[20]*N))
            xdr1 = DataRange1d()
            ydr1 = DataRange1d()
            plot1 = Plot(title="Plot1", x_range=xdr1, y_range=ydr1, plot_width=400, plot_height=400)
            plot1.tools.append(TapTool(plot=plot1))
            plot1.add_glyph(source1, Circle(x="x", y="y", size="size", fill_color="red"))

            source2 = ColumnDataSource(dict(x=x, y=y, color=["blue"]*N))
            xdr2 = DataRange1d()
            ydr2 = DataRange1d()
            plot2 = Plot(title="Plot2", x_range=xdr2, y_range=ydr2, plot_width=400, plot_height=400)
            plot2.tools.append(TapTool(plot=plot2))
            plot2.add_glyph(source2, Circle(x="x", y="y", size=20, fill_color="color"))


            def on_selection_change1(obj, attr, inds):
                color = ["blue"]*N
                if inds['1d']['indices']:
                    [index] = inds['1d']['indices']
                    color[index] = "red"
                source2.data["color"] = color

            source1.on_change('selected', on_selection_change1)


            def on_selection_change2(obj, attr, inds):
                inds = inds['1d']['indices']
                if inds:
                    [index] = inds
                    size = [10]*N
                    size[index] = 40
                else:
                    size = [20]*N
                source1.data["size"] = size

            source2.on_change('selected', on_selection_change2)

            reset = Button(label="Reset")


            def on_reset_click():
                source1.selected = {
                    '0d': {'flag': False, 'indices': []},
                    '1d': {'indices': []},
                    '2d': {'indices': []}
                }
                source2.selected = {
                    '0d': {'flag': False, 'indices': []},
                    '1d': {'indices': []},
                    '2d': {'indices': []}
                }

            reset.on_click(on_reset_click)

            vbox = VBox(children=[reset])
            hbox = HBox(children=[vbox, plot1, plot2])

            document.add_root(hbox)

            if __name__ == "__main__":
                # open the doc in browser
                session.show()
                # run forever
                session.loop_until_closed()

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

            from bokeh.client import push_session
            from bokeh.models import Plot, DataRange1d, LinearAxis, ColumnDataSource, Grid, Legend
            from bokeh.models.glyphs import Line
            from bokeh.models.widgets import Slider, TextInput, HBox, VBox, Dialog
            from bokeh.plotting import curdoc

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

1. [population_server](https://github.com/chdoig/pydata2015-berlin-bokeh/blob/gh-pages/examples/Server/population_server.py)

    1. original

            from __future__ import print_function

            from math import pi

            from bokeh.browserlib import view
            from bokeh.document import Document
            from bokeh.models.glyphs import Line, Quad
            from bokeh.models import (
                Plot, ColumnDataSource, DataRange1d, FactorRange,
                LinearAxis, CategoricalAxis, Grid, Legend,
                SingleIntervalTicker
            )
            from bokeh.sampledata.population import load_population
            from bokeh.session import Session
            from bokeh.models.widgets import Select, HBox, VBox

            document = Document()
            session = Session()
            session.use_doc('population_server')
            session.load_document(document)

            df = load_population()
            revision = 2012

            year = 2010
            location = "World"

            years = [str(x) for x in sorted(df.Year.unique())]
            locations = sorted(df.Location.unique())

            source_pyramid = ColumnDataSource(data=dict())

            def pyramid():
                xdr = DataRange1d()
                ydr = DataRange1d()

                plot = Plot(title=None, x_range=xdr, y_range=ydr, plot_width=600, plot_height=600)

                xaxis = LinearAxis()
                plot.add_layout(xaxis, 'below')
                yaxis = LinearAxis(ticker=SingleIntervalTicker(interval=5))
                plot.add_layout(yaxis, 'left')

                plot.add_layout(Grid(dimension=0, ticker=xaxis.ticker))
                plot.add_layout(Grid(dimension=1, ticker=yaxis.ticker))

                male_quad = Quad(left="male", right=0, bottom="groups", top="shifted", fill_color="#3B8686")
                male_quad_glyph = plot.add_glyph(source_pyramid, male_quad)

                female_quad = Quad(left=0, right="female", bottom="groups", top="shifted", fill_color="#CFF09E")
                female_quad_glyph = plot.add_glyph(source_pyramid, female_quad)

                plot.add_layout(Legend(legends=[("Male", [male_quad_glyph]), ("Female", [female_quad_glyph])]))

                return plot

            source_known = ColumnDataSource(data=dict(x=[], y=[]))
            source_predicted = ColumnDataSource(data=dict(x=[], y=[]))

            def population():
                xdr = FactorRange(factors=years)
                ydr = DataRange1d()

                plot = Plot(title=None, x_range=xdr, y_range=ydr, plot_width=800, plot_height=200)

                plot.add_layout(CategoricalAxis(major_label_orientation=pi/4), 'below')

                line_known = Line(x="x", y="y", line_color="violet", line_width=2)
                line_known_glyph = plot.add_glyph(source_known, line_known)

                line_predicted = Line(x="x", y="y", line_color="violet", line_width=2, line_dash="dashed")
                line_predicted_glyph = plot.add_glyph(source_predicted, line_predicted)

                plot.add_layout(
                    Legend(
                        orientation="bottom_right",
                        legends=[("known", [line_known_glyph]), ("predicted", [line_predicted_glyph])],
                    )
                )

                return plot

            def update_pyramid():
                pyramid = df[(df.Location == location) & (df.Year == year)]

                male = pyramid[pyramid.Sex == "Male"]
                female = pyramid[pyramid.Sex == "Female"]

                total = male.Value.sum() + female.Value.sum()

                male_percent = -male.Value/total
                female_percent = female.Value/total

                groups = male.AgeGrpStart.tolist()
                shifted = groups[1:] + [groups[-1] + 5]

                source_pyramid.data = dict(
                    groups=groups,
                    shifted=shifted,
                    male=male_percent,
                    female=female_percent,
                )

            def update_population():
                population = df[df.Location == location].groupby(df.Year).Value.sum()
                aligned_revision = revision//10 * 10

                known = population[population.index <= aligned_revision]
                predicted = population[population.index >= aligned_revision]

                source_known.data = dict(x=known.index.map(str), y=known.values)
                source_predicted.data = dict(x=predicted.index.map(str), y=predicted.values)

            def update_data():
                update_population()
                update_pyramid()
                session.store_document(document)

            def on_year_change(obj, attr, old, new):
                global year
                year = int(new)
                update_data()

            def on_location_change(obj, attr, old, new):
                global location
                location = new
                update_data()

            def layout():
                year_select = Select(title="Year:", value="2010", options=years)
                location_select = Select(title="Location:", value="World", options=locations)

                year_select.on_change('value', on_year_change)
                location_select.on_change('value', on_location_change)

                controls = HBox(children=[year_select, location_select])
                layout = VBox(children=[controls, pyramid(), population()])

                return layout

            document.add(layout())
            update_data()

            if __name__ == "__main__":
                link = session.object_link(document.context)
                print("Please visit %s to see the plots" % link)
                view(link)
                print("\npress ctrl-C to exit")
                session.poll_document(document)

    1. convert to bokeh 0.11

            from math import pi

            from bokeh.client import push_session
            from bokeh.models.glyphs import Line, Quad
            from bokeh.models.widgets import Select, HBox, VBox
            from bokeh.plotting import curdoc
            from bokeh.sampledata.population import load_population
            from bokeh.models import (
                Plot, ColumnDataSource, DataRange1d, FactorRange,
                LinearAxis, CategoricalAxis, Grid, Legend,
                SingleIntervalTicker
            )

            document = curdoc()
            session = push_session(document)


            df = load_population()
            revision = 2012

            year = 2010
            location = "World"

            years = [str(x) for x in sorted(df.Year.unique())]
            locations = sorted(df.Location.unique())

            source_pyramid = ColumnDataSource(data=dict())


            def pyramid():
                xdr = DataRange1d()
                ydr = DataRange1d()

                plot = Plot(title=None, x_range=xdr, y_range=ydr, plot_width=600, plot_height=600)

                xaxis = LinearAxis()
                plot.add_layout(xaxis, 'below')
                yaxis = LinearAxis(ticker=SingleIntervalTicker(interval=5))
                plot.add_layout(yaxis, 'left')

                plot.add_layout(Grid(dimension=0, ticker=xaxis.ticker))
                plot.add_layout(Grid(dimension=1, ticker=yaxis.ticker))

                male_quad = Quad(left="male", right=0, bottom="groups", top="shifted", fill_color="#3B8686")
                male_quad_glyph = plot.add_glyph(source_pyramid, male_quad)

                female_quad = Quad(left=0, right="female", bottom="groups", top="shifted", fill_color="#CFF09E")
                female_quad_glyph = plot.add_glyph(source_pyramid, female_quad)

                plot.add_layout(Legend(legends=[("Male", [male_quad_glyph]), ("Female", [female_quad_glyph])]))

                return plot

            source_known = ColumnDataSource(data=dict(x=[], y=[]))
            source_predicted = ColumnDataSource(data=dict(x=[], y=[]))


            def population():
                xdr = FactorRange(factors=years)
                ydr = DataRange1d()

                plot = Plot(title=None, x_range=xdr, y_range=ydr, plot_width=800, plot_height=200)

                plot.add_layout(CategoricalAxis(major_label_orientation=pi/4), 'below')

                line_known = Line(x="x", y="y", line_color="violet", line_width=2)
                line_known_glyph = plot.add_glyph(source_known, line_known)

                line_predicted = Line(x="x", y="y", line_color="violet", line_width=2, line_dash="dashed")
                line_predicted_glyph = plot.add_glyph(source_predicted, line_predicted)

                plot.add_layout(
                    Legend(
                        location="bottom_right",
                        legends=[("known", [line_known_glyph]), ("predicted", [line_predicted_glyph])],
                    )
                )

                return plot


            def update_pyramid():
                pyramid = df[(df.Location == location) & (df.Year == year)]

                male = pyramid[pyramid.Sex == "Male"]
                female = pyramid[pyramid.Sex == "Female"]

                total = male.Value.sum() + female.Value.sum()

                male_percent = -male.Value/total
                female_percent = female.Value/total

                groups = male.AgeGrpStart.tolist()
                shifted = groups[1:] + [groups[-1] + 5]

                source_pyramid.data = dict(
                    groups=groups,
                    shifted=shifted,
                    male=male_percent,
                    female=female_percent,
                )


            def update_population():
                population = df[df.Location == location].groupby(df.Year).Value.sum()
                aligned_revision = revision//10 * 10

                known = population[population.index <= aligned_revision]
                predicted = population[population.index >= aligned_revision]

                source_known.data = dict(x=known.index.map(str), y=known.values)
                source_predicted.data = dict(x=predicted.index.map(str), y=predicted.values)


            def update_data():
                update_population()
                update_pyramid()


            def on_year_change(attr, old, new):
                global year
                year = int(new)
                update_data()


            def on_location_change(attr, old, new):
                global location
                location = new
                update_data()


            def layout():
                year_select = Select(title="Year:", value="2010", options=years)
                location_select = Select(title="Location:", value="World", options=locations)

                year_select.on_change('value', on_year_change)
                location_select.on_change('value', on_location_change)

                controls = HBox(children=[year_select, location_select])
                layout = VBox(children=[controls, pyramid(), population()])

                return layout

            document.add_root(layout())
            update_data()

            if __name__ == "__main__":
                # open the doc in browser
                session.show()
                # run forever
                session.loop_until_closed()