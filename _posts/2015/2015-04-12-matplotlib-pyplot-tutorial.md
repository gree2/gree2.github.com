---
layout: post
title: "matplotlib pyplot tutorial"
description: ""
category: [python]
tags: [python, matplotlib, pyplot]
---
{% include JB/setup %}


### pyplot tutorial

* `matplotlib.pyplot` is a collection of command style functions that make matplotlib work like MATLAB

* each `pyplot` function makes some change to a figure

    * create a figure

    * create a plotting area in a figure

    * plot some lines in a plotting area

    * decorate the plot with labels

    * etc...

* `matplotlib.pyplot` is stateful

    * it that it keeps track of the current figure and plotting area

    * and the plotting functions are directed to the current axes

### code

1. draw line

            import matplotlib.pyplot as plt
            plt.plot([1, 2, 3, 4])
            plt.ylabel('some numbers')
            plt.show()

    ![pyplot_simple](http://matplotlib.org/_images/pyplot_simple.png)

    * if you provide a single list or array to the `plot()` command, matplotlib assumes it is a sequence of `y` values

    * and automatically generates `x` values for you

    * the default `x` vector has same length as `y` but start with `0`

    * hence the `x` data are `[0, 1, 2, 3]`

1. `plot()` is a versatile command and will take an arbitrary number of arguments

    * to plot `x` versus `y` you can issue the command

            plt.plot([1, 2, 3, 4], [1, 4, 9, 16])

1. for every `x,y` pair of arguments there is an optional third argument

    * which is the format string

        * indicates the color

        * and line type of the plot

    * the letters and symbols of the format string are from `MATLAB`

    * default format string is `b-`, which is a `solid blue line`

            import matplotlib.pyplot as plt
            plt.plot([1, 2, 3, 4], [1, 4, 9, 16], 'ro')
            plt.axis([0, 6, 0, 20])
            plt.show()

    ![pyplot_formatstr](http://matplotlib.org/_images/pyplot_formatstr.png)

    * the `axis()` command in the example above takes a list of `[xmin, xmax, ymin, ymax]`

    * and specifies the viewport of the axes

1. all sequences are converted to numpy arrays internally

            import numpy as np
            import matplotlib.pyplot as plt
            # evenly sampled time at 200ms intervals
            t = np.arange(0., 5., 0.2)
            # red dashes, blue squares and green triangles
            plt.plot(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
            plt.show()

    ![pyplot_three](http://matplotlib.org/_images/pyplot_three.png)

### controlling line properties

1. line have many attributes that you can set

    1. linewidth

    1. dash style

    1. antialiased

1. there are several ways to set line properties

    1. use keyword args

            plt.plot(x, y, linewidth=2.0)

    2. use the setter methods of the `Line2D` instance `.plot` returns a list of lines

            line1, line2 = plot(x1, y1, x2, y2)

            line, = plt.plot(x, y, '-')
            # turn off antialising
            line.set_antialiased(False)

    3. use the `setp()` command

            lines = plt.plot(x1, y1, x2, y2)
            # use keyword args
            plt.setp(lines, color='r', linewidth=2.0)
            # or MATLAB style string values pairs
            plt.setp(lines, 'color', 'r', 'linewidth', 2.0)

### Line2D properties

<table border="1" class="docutils">
  <colgroup>
  <col width="24%" />
  <col width="76%" />
  </colgroup>
  <thead valign="bottom">
    <tr class="row-odd"><th class="head">Property</th>
    <th class="head">Value Type</th>
  </tr>
</thead>
<tbody valign="top">
  <tr class="row-even"><td>alpha</td>
  <td>float</td>
</tr>
<tr class="row-odd"><td>animated</td>
<td>[True | False]</td>
</tr>
<tr class="row-even"><td>antialiased or aa</td>
<td>[True | False]</td>
</tr>
<tr class="row-odd"><td>clip_box</td>
<td>a matplotlib.transform.Bbox instance</td>
</tr>
<tr class="row-even"><td>clip_on</td>
<td>[True | False]</td>
</tr>
<tr class="row-odd"><td>clip_path</td>
<td>a Path instance and a Transform instance, a Patch</td>
</tr>
<tr class="row-even"><td>color or c</td>
<td>any matplotlib color</td>
</tr>
<tr class="row-odd"><td>contains</td>
<td>the hit testing function</td>
</tr>
<tr class="row-even"><td>dash_capstyle</td>
<td>[<tt class="docutils literal"><span class="pre">'butt'</span></tt> | <tt class="docutils literal"><span class="pre">'round'</span></tt> | <tt class="docutils literal"><span class="pre">'projecting'</span></tt>]</td>
</tr>
<tr class="row-odd"><td>dash_joinstyle</td>
<td>[<tt class="docutils literal"><span class="pre">'miter'</span></tt> | <tt class="docutils literal"><span class="pre">'round'</span></tt> | <tt class="docutils literal"><span class="pre">'bevel'</span></tt>]</td>
</tr>
<tr class="row-even"><td>dashes</td>
<td>sequence of on/off ink in points</td>
</tr>
<tr class="row-odd"><td>data</td>
<td>(np.array xdata, np.array ydata)</td>
</tr>
<tr class="row-even"><td>figure</td>
<td>a matplotlib.figure.Figure instance</td>
</tr>
<tr class="row-odd"><td>label</td>
<td>any string</td>
</tr>
<tr class="row-even"><td>linestyle or ls</td>
<td>[ <tt class="docutils literal"><span class="pre">'-'</span></tt> | <tt class="docutils literal"><span class="pre">'--'</span></tt> | <tt class="docutils literal"><span class="pre">'-.'</span></tt> | <tt class="docutils literal"><span class="pre">':'</span></tt> | <tt class="docutils literal"><span class="pre">'steps'</span></tt> | ...]</td>
</tr>
<tr class="row-odd"><td>linewidth or lw</td>
<td>float value in points</td>
</tr>
<tr class="row-even"><td>lod</td>
<td>[True | False]</td>
</tr>
<tr class="row-odd"><td>marker</td>
<td>[ <tt class="docutils literal"><span class="pre">'+'</span></tt> | <tt class="docutils literal"><span class="pre">','</span></tt> | <tt class="docutils literal"><span class="pre">'.'</span></tt> | <tt class="docutils literal"><span class="pre">'1'</span></tt> | <tt class="docutils literal"><span class="pre">'2'</span></tt> | <tt class="docutils literal"><span class="pre">'3'</span></tt> | <tt class="docutils literal"><span class="pre">'4'</span></tt> ]</td>
</tr>
<tr class="row-even"><td>markeredgecolor or mec</td>
<td>any matplotlib color</td>
</tr>
<tr class="row-odd"><td>markeredgewidth or mew</td>
<td>float value in points</td>
</tr>
<tr class="row-even"><td>markerfacecolor or mfc</td>
<td>any matplotlib color</td>
</tr>
<tr class="row-odd"><td>markersize or ms</td>
<td>float</td>
</tr>
<tr class="row-even"><td>markevery</td>
<td>[ None | integer | (startind, stride) ]</td>
</tr>
<tr class="row-odd"><td>picker</td>
<td>used in interactive line selection</td>
</tr>
<tr class="row-even"><td>pickradius</td>
<td>the line pick selection radius</td>
</tr>
<tr class="row-odd"><td>solid_capstyle</td>
<td>[<tt class="docutils literal"><span class="pre">'butt'</span></tt> | <tt class="docutils literal"><span class="pre">'round'</span></tt> | <tt class="docutils literal"><span class="pre">'projecting'</span></tt>]</td>
</tr>
<tr class="row-even"><td>solid_joinstyle</td>
<td>[<tt class="docutils literal"><span class="pre">'miter'</span></tt> | <tt class="docutils literal"><span class="pre">'round'</span></tt> | <tt class="docutils literal"><span class="pre">'bevel'</span></tt>]</td>
</tr>
<tr class="row-odd"><td>transform</td>
<td>a matplotlib.transforms.Transform instance</td>
</tr>
<tr class="row-even"><td>visible</td>
<td>[True | False]</td>
</tr>
<tr class="row-odd"><td>xdata</td>
<td>np.array</td>
</tr>
<tr class="row-even"><td>ydata</td>
<td>np.array</td>
</tr>
<tr class="row-odd"><td>zorder</td>
<td>any number</td>
</tr>
</tbody>
</table>

### working with multiple figures and axes

* MATLAB and `pyplot` have the concept of the current figure and the current axes

* all plotting commands apply to the current axes

* the function `gca()` returns the current axes (a `matplotlib.axes.Axes` instance)

* and `gcf()` returns the current figure (`matplotlib.figure.Figure` instance)

            import numpy as np
            import matplotlib.pyplot as plt

            def f(t):
                return np.exp(-t) * np.cos(2 * np.pi * t)

            t1 = np.arange(0.0, 5.0, 0.1)
            t1 = np.arange(0.0, 5.0, 0.02)

            plt.figure(1)
            plt.subplot(211)
            plt.plot(t1, f(t1), 'bo', t2, f(t2), 'k')

            plt.subplot(212)
            plt.plot(t2, np.cos(2 * np.pi * t2), 'r--')
            plt.show()

    ![pyplot_two_subplots](http://matplotlib.org/_images/pyplot_two_subplots.png)

* you can create multiple figures by using multiple `figure()` calls with an increasing figure number

            import matplotlib.pyplot as plt
            plt.figure(1)                  # the first figure
            plt.subplot(211)               # the first subplot in the first figure
            plt.plot([1, 2, 3])
            plt.subplot(212)               # the second subplot in the first figure
            plt.plot([4, 5, 6])

            plt.figure(2)                  # the second figure
            plt.plot([4, 5, 6])            # creates a subplot(111) by default

            plt.figure(1)                  # figure 1 current; subplot(212) still current
            plt.subplot(211)               # make subplot(211) in figure 1 current
            plt.title('easy as 1, 2, 3')   # subplot 211 title

* you can clear the current figure with `clf()` and the current axes with `cla()`

* a figure is not completely released until the figure is explicitly closed with `close()`

### working with text

* the `text()` command can be used to add text in an arbitrary location

* the `xlabel(), ylabel()` and `title()` are used to add text in the indicated locations

            import numpy as np
            import matplotlib.pyplot as plt

            mu, sigma = 100, 15
            x = mu + sigma * np.random.randn(10000)

            # the histogram of the data
            n, bins, patches = plt.hist(x, 50, normed=1, facecolor='g', alpha=0.75)


            plt.xlabel('Smarts')
            plt.ylabel('Probability')
            plt.title('Histogram of IQ')
            plt.text(60, .025, r'$\mu=100,\ \sigma=15$')
            plt.axis([40, 160, 0, 0.03])
            plt.grid(True)
            plt.show()

    ![pyplot_text](http://matplotlib.org/_images/pyplot_text.png)

* all the `text()` commands return an `matplotlib.text.Text` instance

            t = plt.xlabel('my data', fontsize=14, color='red')

    * you can customize the properties by passing keyword arguments into the text functions or using `setp()`

### annotating text

* `annotate()` method provides helper functionality to make annotation ease

* there 2 points to consider

    * the location being annotated represented by the argument `xy`

    * the location of the text `xytext`

            import numpy as np
            import matplotlib.pyplot as plt

            ax = plt.subplot(111)

            t = np.arange(0.0, 5.0, 0.01)
            s = np.cos(2*np.pi*t)
            line, = plt.plot(t, s, lw=2)

            plt.annotate('local max', xy=(2, 1), xytext=(3, 1.5),
                        arrowprops=dict(facecolor='black', shrink=0.05),
                        )

            plt.ylim(-2,2)
            plt.show()

    ![pyplot_annotate](http://matplotlib.org/_images/pyplot_annotate.png)

    * see [annotating text](http://matplotlib.org/users/annotations_intro.html#annotations-tutorial) and [annotating axes](http://matplotlib.org/users/annotations_guide.html#plotting-guide-annotation)

    * more examples can be found in [pylab-examples-annotation-demo](http://matplotlib.org/examples/pylab_examples/annotation_demo.html#pylab-examples-annotation-demo)