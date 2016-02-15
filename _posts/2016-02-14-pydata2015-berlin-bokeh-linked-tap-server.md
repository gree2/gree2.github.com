---
layout: post
title: "pydata2015 berlin bokeh linked tap server"
description: ""
category: [viz, python]
tags: [bokeh, server, datarange, selected, taptool]
---
{% include JB/setup %}


### code

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

            from bokeh.io import curdoc
            from bokeh.client import push_session
            from bokeh.models import ColumnDataSource, DataRange1d, Plot, Circle, VBox, HBox, Button, TapTool

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

### [add_root(model)](http://bokeh.pydata.org/en/latest/docs/reference/document.html#bokeh.document.Document.add_root)

1. bokeh.document.Document.add_root

1. description

    1. add a model as a root model to this document

    1. any change to this model (including to other models referred to by it) will trigger `on_change` callbacks registered on this document


### [class DataRange1d(*args, **kwargs)](http://bokeh.pydata.org/en/latest/docs/reference/models/ranges.html#bokeh.models.ranges.DataRange1d)

1. base bokeh.models.ranges.DataRange

1. description

    1. an auto-fitting range in a continuous scalar dimension

    1. the upper and the lower bounds are set to the min and the max of the data

### [class Plot(**kwargs)](http://bokeh.pydata.org/en/latest/docs/reference/models/plots.html#bokeh.models.plots.Plot)

1. base bokeh.models.component.Component

1. description

    1. model representing a plot, containing glyphs, guides, annotations

1. methods

    1. [add_glyph(source_or_glyph, glyph=None, **kw)](http://bokeh.pydata.org/en/latest/docs/reference/models/plots.html#bokeh.models.plots.Plot.add_glyph)

            add a glyph to the plot with associated data sources and ranges

            parameters: source (DataSource) a data source for the glyphs to use
                        glyph (Glyph) the glyph to add to the plot
            keyword arguments: any additional keyword arguments are passed on as-is to the Glyph initializer

1. attr

    1. [tools](http://bokeh.pydata.org/en/latest/docs/reference/models/plots.html#bokeh.models.plots.Plot.tools)

            a list of tools to add to the plot

### [class ColumnDataSource(*args, **kw)](http://bokeh.pydata.org/en/latest/docs/reference/models/sources.html#bokeh.models.sources.ColumnDataSource)

1. base bokeh.models.sources.DataSource

1. description

    1. maps names of columns to sequences or arrays

            ColumnDataSource(mydict) # same as ColumnDataSource(data=mydict)
            ColumnDataSource(df)     # same as ColumnDataSource(data=df)

1. attr

    1. [selected](http://bokeh.pydata.org/en/latest/docs/reference/models/sources.html#bokeh.models.sources.DataSource.selected)

            a dict to indicate selected indices on different dimensions on this DataSource
            
            keys are:

            0d: indicates whether a line or patch glyphs have been hit

                value is a dict with the following keys

                flag (boolean): true if glyph was with false otherwise
                indices (list): indices hit (if applicable)

            1d: indicates whether any of all other glyph (except line multiline or patches) was hit

                value is a dict with the following keys

                indices (list): indices that were hit/selected

            2d: indicates whether a line multiline or patches were hit

                value is a dict with the following keys

                indices (list): indices of the line/patches that were hit/selected