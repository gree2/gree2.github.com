---
layout: post
title: "python seaborn tutorial choosing color palettes"
description: ""
category: [python]
tags: [seaborn, color palettes, circular, sequential, diverging]
---
{% include JB/setup %}


### choosing color palettes

1. building color palettes with `color_palette()

1. qualitative color palettes

1. using circular color systems

1. using categorical color brewer palettes

1. using names colors from the xkcd color survey

1. sequential color palettes

1. sequential palettes with `cubehelix_palette()`

1. custom sequential palettes with `light_palette()` and `dark_palette()`

1. diverging color palettes

1. custom diverging palettes with `diverging_palette()`

1. changing default palettess with `set_palette()`

### import

            %matplotlib inline
            import numpy as np
            import seaborn as sns
            import matplotlib.pyplot as plt

            sns.set(rc={'figure.figsize': (6, 6)})
            np.random.seed(sum(map(ord, 'palettes')))

### building color palettes with `color_palette()`

`color_palette()` accept the name of any `seaborn palette` or `matplotlib colormap`

except `jet` which you should never use

also take a list of colors specified in any valid matplotlib format

* rgb tuples

* hex color codes

* html color names

the return value is alays a list of rgb tuples

calling `color_palette()` with no arguments will return the current default color cycle

### qualitative color palettes

* qualitative or categorical palettes

* there are six variations of the default theme

    1. deep

    1. muted

    1. pastel

    1. bright

    1. dark

    1. colorblind

            current_palette = sns.color_palette()
            sns.palplot(current_palette)

### using circular color systems

when you have more than six categories to distinguish

the most common way to do this uses the `hls` color space

which is a simple transformation of rgb values

            sns.palplot(sns.color_palette('hls', 8))

            # control the `lightness` and `saturation` of the colors
            sns.palplot(sns.hls_palette(8, l=.3, s=.8))

            # `husl` system
            sns.palplot(sns.color_palette('husl', 8))

            sns.palplot(sns.husl_palette(8))

### using categorical color brewer palettes

[color brewer tool](http://colorbrewer2.org/)

which also has sequential and diverging palettes

            sns.palplot(sns.color_palette('Paired'))

            sns.palplot(sns.color_palette('Set2', 10))

            flatui = ["#9b59b6", "#3498db", "#95a5a6", "#e74c3c", "#34495e", "#2ecc71"]
            sns.palplot(sns.color_palette(flatui))

### using named color from the `xkcd` color survey

[xkcd](http://xkcd.com/) ran a [crowdsourced effort](http://blog.xkcd.com/2010/05/03/color-survey-results/) to name random rgb color

this produced a set of [954 named colors](http://xkcd.com/color/rgb/)

using `xkcd_rgb` dictionary

            plt.plot([0, 1], [0, 1], sns.xkcd_rgb['pale red'], lw=3)
            plt.plot([0, 1], [0, 2], sns.xkcd_rgb['medium green'], lw=3)
            plt.plot([0, 1], [0, 3], sns.xkcd_rgb['denim blue'], lw=3)

            colors = ["windows blue", "amber", "greyish", "faded green", "dusty purple"]
            sns.palplot(sns.xkcd_palette(colors))

### sequential color palettes

more common to use them as a colormap in functions like `kdeplot()` or `corrplot`

            sns.palplot(sns.color_palette('Blues'))

            # reversed lightness ramp add a `_r` suffix
            sns.palplot(sns.color_palette('BuGn_r'))

            # seaborn also adds a trick that allows you to create `dark` palettes
            sns.palplot(sns.color_palette('GnBu_d'))

### sequential palettes with `cubehelix_palette()`

the [cubehelix](http://www.mrao.cam.ac.uk/~dag/CUBEHELIX/) color palette system makes sequential palettes

with a linear increase or decrease in brightness and some variation in hue

matplotlib has the default cubehlix version built into it

            sns.palplot(sns.color_palette('cubehelix', 8))

            sns.palplot(sns.cubehelix_palette(8))

            # 0 < `start` < 3
            # -1 < `rot` < 1
            sns.palplot(sns.cubehelix_palette(8, start=.5, rot=-.75))

            # how dark and light the endpoints
            # reverse the ramp
            sns.palplot(sns.cubehelix_palette(8, start=2, rot=0, dark=0, light=.95, reverse=True))

            # using `as_cmap=True` return the palette as a colormap object
            # that can be passed to seaborn or matplotlib functions
            x, y = np.random.multivariate_normal([0 ,0], [[1, -.5], [-.5, 1]], size=300).T
            cmap = sns.cubehelix_palette(light=1, as_cmap=True)
            sns.kdeplot(x, y, cmap=cmap, shade=True)

### custom sequential palettes with `light_palette()` and `dark_palette()`

            sns.palplot(sns.light_palette('green'))

            sns.palplot(sns.dark_palette('purple'))

            # reversed
            sns.palplot(sns.light_palette('navy', reverse=True))

            # create colormap objects rather than lists of colors
            pal = sns.dark_palette('palegreen', as_cmap=True)
            sns.kdeplot(x, y, cmap=pal)

            sns.palplot(sns.light_palette((210, 90, 60), input='husl'))

            sns.palplot(sns.dark_palette('muted purple', input='xkcd'))

### diverging color palettes

used for data where both large low and high values are interesting

            sns.palplot(sns.color_palette('BrBG', 7))

            sns.palplot(sns.color_palette('RdBu_r', 7))

            sns.palplot(sns.color_palette('coolwarm', 7))

### custom diverging palettes with `diverging_palette()`

            sns.palplot(sns.diverging_palette(220, 20, n=7))

            sns.palplot(sns.diverging_palette(145, 280, s=85, l=25, n=7))

            # `sep` argument controls the width of the separation between the two ramps
            sns.palplot(sns.diverging_palette(10, 220, sep=80, n=7))

            # make midpoint darker
            sns.palplot(sns.diverging_palette(255, 133, l=60, n=7, center='dark'))

### changing default palettes with `set_palette()`

            def sinplot(flip=1):
                x = np.linspace(0, 14, 100)
                for i in range(1, 7):
                    plt.plot(x, np.sin(x + i * .5) * (7 - i) * flip)

            sns.set_palette('husl')
            sinplot()

            # in `with` statement
            with sns.color_palette('PuBuGn_d'):
                sinplot()