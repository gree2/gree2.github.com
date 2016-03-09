---
layout: post
title: "python seaborn tutorial controlling figure aesthetics"
description: ""
category: [python]
tags: [seaborn, figure, style, context]
---
{% include JB/setup %}


### contents

1. styling figures with axes_style() and set_style()

1. removing spines with despine()

1. temporarity setting figure style

1. overriding element of the seaborn styles

1. scaling plot elements with plotting_context() and set_context()

### code

1. import

            %matplotlib inline

            import numpy as np
            import matplotlib as mpl
            import matplotlib.pyplot as plt

            # 1069 is sum(map(ord, 'aesthetics))
            np.random.seed(sum(map(ord, 'aesthetics')))

            def sinplot(flip=1):
                x = np.linspace(0, 14, 100)
                for i in range(1, 7):
                    plt.plot(x, np.sin(x + i * .5) * (7 - i) * flip)

            sinplot()

1. import seaborn

            import seaborn as sns
            sinplot()

### styling figures with axes_style() and set_style()

* preset seaborn themes

    1. `darkgrid => default`

    1. whitegrid

    1. dark

    1. white

    1. ticks

1. whitegrid

            sns.set_style('whitegrid')
            data = np.random.normal(size=(20, 6)) + np.arange(6) / 2
            sns.boxplot(data)

1. dark

            sns.set_style('dark')
            sinplot()

1. white

            sns.set_style('white')
            sinplot()

1. ticks

            sns.set_style('ticks')
            sinplot()

### removing spines with despine()

* both `white` and `ticks` styles can benefit from removing the top and right axes splines

* through the matplotlib parameters or call `despine()`

1. code

            sinplot()
            sns.despine()

1. trim range

            # offsetting the spines away form the data
            # `trim` will limit the range of the surviving spines
            f, ax = plt.subplots()
            sns.violinplot(data)
            sns.despine(offset=10, trim=True)

1. control which spines are removed

            # control which spines are removed
            sns.set_style('whitegrid')
            sns.boxplot(data, color='deep')
            sns.despine(left=True)

### temporarity setting figure style

* use `axes_style()` function in a `with` statement to temporarily set plot parameters

            with sns.axes_style('darkgrid'):
                plt.subplot(211)
                sinplot()
                
            plt.subplot(212)
            sinplot(-1)

### overriding element of the seaborn styles

* pass a dictionary of parameters to the `rc` argument

1. get parameters

            # get parameters
            sns.axes_style()

            {'axes.axisbelow': True,
             'axes.edgecolor': '.8',
             'axes.facecolor': 'white',
             'axes.grid': True,
             'axes.labelcolor': '.15',
             'axes.linewidth': 1.0,
             'font.family': [u'sans-serif'],
             'font.sans-serif': [u'Arial',
              u'Liberation Sans',
              u'Bitstream Vera Sans',
              u'sans-serif'],
             'grid.color': '.8',
             'grid.linestyle': u'-',
             'image.cmap': u'Greys',
             'legend.frameon': False,
             'legend.numpoints': 1,
             'legend.scatterpoints': 1,
             'lines.solid_capstyle': u'round',
             'text.color': '.15',
             'xtick.color': '.15',
             'xtick.direction': u'out',
             'xtick.major.size': 0.0,
             'xtick.minor.size': 0.0,
             'ytick.color': '.15',
             'ytick.direction': u'out',
             'ytick.major.size': 0.0,
             'ytick.minor.size': 0.0}

1. set parameters

            # then set different versions of these parameters
            sns.set_style('darkgrid', {'grid.linewidth': .5,
                                       'axes.facecolor': '.9'})
            sinplot()

### scaling plot elements with plotting_context() and set_context()

* preset scaling

    1. paper

    1. `notebook => default`

    1. talk

    1. poster

1. reset

            # reset the default parameters
            sns.set()

1. paper

            sns.set_context('paper')
            plt.figure(figsize=(8, 6))
            sinplot()

1. talk

            sns.set_context('talk')
            plt.figure(figsize=(8, 6))
            sinplot()

1. poster

            sns.set_context('paper')
            plt.figure(figsize=(8, 6))
            sinplot()

1. notebook

            sns.set_context('notebook', font_scale=1.5, rc={'line.linewidth': 2.5})
            sinplot()