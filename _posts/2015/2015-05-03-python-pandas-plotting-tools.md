---
layout: post
title: "python pandas plotting tools"
description: ""
category: [python]
tags: [pandas, plotting, scatter, desity, andrews curves, parallel coordinates, lag plot, autocorrelation, bootstrap, radviz]
---
{% include JB/setup %}


### plotting tools

1. [scatter matrix plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#scatter-matrix-plot)

1. [desity plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#density-plot)

1. [andrews curves](http://pandas.pydata.org/pandas-docs/stable/visualization.html#andrews-curves)

1. [parallel coordinates](http://pandas.pydata.org/pandas-docs/stable/visualization.html#parallel-coordinates)

1. [lag plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#lag-plot)

1. [autocorrelation plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#autocorrelation-plot)

1. [bootstrap plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#bootstrap-plot)

1. [radviz](http://pandas.pydata.org/pandas-docs/stable/visualization.html#radviz)

### code

* import code

            import matplotlib.pyplot as plt
            import matplotlib
            matplotlib.style.use('ggplot')
            import numpy as np
            import pandas as pd
            %matplotlib inline

1. [scatter matrix plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#scatter-matrix-plot)

            from pandas.tools.plotting import scatter_matrix
            df = pd.DataFrame(np.random.randn(1000, 4),
                              columns=['a', 'b', 'c', 'd'])
            scatter_matrix(df, alpha=0.2, figsize=(6, 6), diagonal='kde')

1. [desity plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#density-plot)

            ser = pd.Series(np.random.randn(1000))
            ser.plot(kind='kde')

1. [andrews curves](http://pandas.pydata.org/pandas-docs/stable/visualization.html#andrews-curves)

            from pandas import read_csv
            from pandas.tools.plotting import andrews_curves
            data = pd.read_csv('iris.data')
            plt.figure()
            andrews_curves(data, 'Name')

1. [parallel coordinates](http://pandas.pydata.org/pandas-docs/stable/visualization.html#parallel-coordinates)

            from pandas import read_csv
            from pandas.tools.plotting import parallel_coordinates
            data = read_csv('iris.data')
            plt.figure()
            parallel_coordinates(data, 'Name')

1. [lag plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#lag-plot)

            from pandas.tools.plotting import lag_plot
            plt.figure()
            data = pd.Series(0.1 * np.random.rand(1000) +
                             0.9 * np.sin(np.linspace(-99 * np.pi, 99 * np.pi, num=1000)))
            lag_plot(data)

1. [autocorrelation plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#autocorrelation-plot)

            from pandas.tools.plotting import autocorrelation_plot
            plt.figure()
            data = pd.Series(0.7 * np.random.rand(1000) +
                             0.3 * np.sin(np.linspace(-9 * np.pi, 9 * np.pi, num=1000)))
            autocorrelation_plot(data)

1. [bootstrap plot](http://pandas.pydata.org/pandas-docs/stable/visualization.html#bootstrap-plot)

            from pandas.tools.plotting import bootstrap_plot
            data = pd.Series(np.random.rand(1000))
            bootstrap_plot(data, size=50, samples=500, color='grey')

1. [radviz](http://pandas.pydata.org/pandas-docs/stable/visualization.html#radviz)

            from pandas import read_csv
            from pandas.tools.plotting import radviz
            data = read_csv('iris.data')
            plt.figure()
            radviz(data, 'Name')