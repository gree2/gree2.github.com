---
layout: post
title: "python pandas plotting other plot"
description: ""
category: [python]
tags: [bar, histogram, box, area, scatter, hexagonal, pie]
---
{% include JB/setup %}


### other plots

1. [bar plots](http://pandas.pydata.org/pandas-docs/stable/visualization.html#bar-plots)

1. [histograms](http://pandas.pydata.org/pandas-docs/stable/visualization.html#histograms)

1. [box plots](http://pandas.pydata.org/pandas-docs/stable/visualization.html#box-plots)

1. [area plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#area-plot)

1. [scatter plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#scatter-plot)

1. [hexagonal bin plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#hexagonal-bin-plot)

1. [pie plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#pie-plot)

### code

* import and basic

    * import

            import matplotlib.pyplot as plt
            import matplotlib
            matplotlib.style.use('ggplot')
            import numpy as np
            import pandas as pd
            %matplotlib inline

    * basic

            # 
            data = np.random.randn(1000)
            days = pd.date_range('2000-01-01', periods=1000)
            ts = pd.Series(data, index=days)
            ts = ts.cumsum()
            # the plot method on Series and DataFrame
            # is just a simple wrapper around plt.plot()
            ts.plot()

            # 
            data = np.random.randn(1000, 4)
            df = pd.DataFrame(data, index=ts.index, columns=list('ABCD'))
            df = df.cumsum()
            plt.figure(); df.plot();


1. bar plots

    * basic

            plt.figure()
            # calling a df's plot() method
            # with `kind='bar'` produces a multiple bar plot
            df.ix[5].plot(kind='bar'); plt.axhline(0, color='k')

            df2 = pd.DataFrame(np.random.rand(10, 4), columns=['a', 'b', 'c', 'd'])
            df2.plot(kind='bar');

    * stasked bar

            # to produce a stacked bar plot
            # pass `stacked=True`
            df2.plot(kind='bar', stacked=True)

    * horizontal bar

            # to get horizontal bar plots
            # pass `kind='barh'`
            df2.plot(kind='barh', stacked=True)

1. histograms

    * basic

            df4 = pd.DataFrame({'a': np.random.randn(1000) + 1,
                               'b': np.random.randn(1000),
                               'c': np.random.randn(1000) - 1}, columns=['a', 'b', 'c'])
            plt.figure();
            df4.plot(kind='hist', alpha=0.5)

    * stacked histogram

            # histogram can be stacked by `stacked=True`
            # bin size can be changed by `bins` keyword
            plt.figure();
            df4.plot(kind='hist', stacked=True, bins=20)

    * horizontal and cumulative

            # horizontal by orientation='horizontal'
            # cumulative by cumulative=True
            plt.figure();
            df4['a'].plot(kind='hist', orientation='horizontal', cumulative=True)

            plt.figure();
            df['A'].diff().hist()

    * color alpha

            plt.figure()
            df.diff().hist(color='k', alpha=0.5, bins=50)

            data = pd.Series(np.random.randn(1000))
            data.hist(by=np.random.randint(0, 4, 1000), figsize=(6, 4))


1. box plots

    * basic

            # five trials of 10 observations of
            # a uniform random variable on [0, 1)
            df = pd.DataFrame(np.random.rand(10, 5), columns=['A', 'B', 'C', 'D', 'E'])
            df.plot(kind='box')

            color = dict(boxes='DarkGreen',
                        whiskers='DarkOrange',
                        medians='DarkBlue',
                        caps='Gray')
            df.plot(kind='box', color=color, sym='r+')

    * vert and positions

            # vert=False and positions
            df.plot(kind='box', vert=False, positions=[1, 2, 5, 6, 8])

            df = pd.DataFrame(np.random.rand(10, 5))
            plt.figure();
            bp = df.boxplot()

    * by

            # using `by` keyword
            df = pd.DataFrame(np.random.rand(10, 2), columns=['col1', 'col2'])
            df['X'] = pd.Series(['A', 'A', 'A', 'A', 'A', 'B', 'B', 'B', 'B', 'B'])
            plt.figure()
            bp = df.boxplot(by='X')

            # pass a subset of colomns to plot
            # as well as group by multiple columns
            df = pd.DataFrame(np.random.rand(10, 3), columns=['col1', 'col2', 'col3'])
            df['X'] = pd.Series(['A', 'A', 'A', 'A', 'A', 'B', 'B', 'B', 'B', 'B'])
            df['Y'] = pd.Series(['A', 'B', 'A', 'B', 'A', 'B', 'A', 'B', 'A', 'B'])
            plt.figure();
            bp = df.boxplot(column=['col1', 'col2'], by=['X', 'Y'])

    * groupby

            # when calling boxplot on a `groupby` object
            # a dict of `return_type` is returned
            np.random.seed(1234)
            df_box = pd.DataFrame(np.random.randn(50, 2))
            df_box['g'] = np.random.choice(['A', 'B'], size=50)
            df_box.loc[df_box['g'] == 'B', 1] += 3
            bp = df_box.boxplot(by='g')

            bp = df_box.groupby('g').boxplot()

1. area plot

    * basic

            df = pd.DataFrame(np.random.rand(10, 4), columns=['a', 'b', 'c', 'd'])
            df.plot(kind='area')

    * stacked area

            # pass `stacked=False`
            # `alpha` value is set to 0.5
            df.plot(kind='area', stacked=False)

1. scatter plot

    * basic

            df = pd.DataFrame(np.random.rand(50, 4), columns=['a', 'b', 'c', 'd'])
            df.plot(kind='scatter', x='a', y='b')

    * multiple column groups

            # to plot multiple column groups in a single axes
            # repeat `plot` method specifying target `ax`
            # recommended to specify `color` and `label` keywords
            # to distinguish each groups
            ax = df.plot(kind='scatter', x='a', y='b',
                         color='DarkBlue', label='Group 1');
            df.plot(kind='scatter', x='c', y='d',
                    color='DarkGreen', label='Group 2', ax=ax);

    * c and s keyword

            # keyword `c` may be given as the name of a column
            # to provide colors for each point
            df.plot(kind='scatter', x='a', y='b', c='c', s=50)

    * bubble size

            # show bubble chart using a df's column as bubble size
            df.plot(kind='scatter', x='a', y='b', s=df.c*200)

1. hexagonal bin plot

    * basic

            df = pd.DataFrame(np.random.randn(1000, 2), columns=['a', 'b'])
            df.b = df.b + np.arange(1000)
            # `gridsize` controls the number of hexagons in the `x-direction`
            # default value is `100`
            # a larger `gridsize` means more, smaller bins
            df.plot(kind='hexbin', x='a', y='b', gridsize=25)

    * C and reduce_C_function

            df = pd.DataFrame(np.random.randn(1000, 2), columns=['a', 'b'])
            df.b = df.b + np.arange(1000)
            df['z'] = np.random.uniform(0, 3, 1000)
            df.plot(kind='hexbin', x='a', y='b', 
                    C='z', reduce_C_function=np.max, gridsize=25)

1. pie plot

    * basic

            series = pd.Series(3 * np.random.rand(4),
                               index=['a', 'b', 'c', 'd'],
                               name='series')
            series.plot(kind='pie', figsize=(6, 6))

    * legend and labels

            df = pd.DataFrame(3 * np.random.rand(4, 2),
                              index=['a', 'b', 'c', 'd'],
                              columns=['x', 'y'])
            # `legend=False` to hide legend
            # `labels=None` to hide labels
            df.plot(kind='pie', subplots=True, figsize=(8,4),
                    legend=False, labels=None)

    * percentage and fontsize

            series.plot(kind='pie',
                        labels=['AA', 'BB', 'CC', 'DD'],
                        colors=['r', 'g', 'b', 'c'],
                        autopct='%.2f', fontsize=20, figsize=(6, 6))

    * semicircle

            # pass values whose sum total is less than 1.0
            # matplotlib draws a semicircle
            series = pd.Series([0.1] * 4, name='series2',
                               index=['a', 'b', 'c', 'd'])
            series.plot(kind='pie', figsize=(6, 6))
