---
layout: post
title: "play wordcloud"
description: ""
category: [python]
tags: [jieba, wordcloud, scipy, matplotlib, masked]
---
{% include JB/setup %}


### code

1. check this [demo - auto_examples masked](https://amueller.github.io/word_cloud/auto_examples/masked.html#sphx-glr-auto-examples-masked-py)

    1. code

            from os import path
            from PIL import Image
            import numpy as np
            import matplotlib.pyplot as plt

            from wordcloud import WordCloud, STOPWORDS

            d = path.dirname(__file__)

            # Read the whole text.
            text = open(path.join(d, 'alice.txt')).read()

            # read the mask image
            # taken from
            # http://www.stencilry.org/stencils/movies/alice%20in%20wonderland/255fk.jpg
            alice_mask = np.array(Image.open(path.join(d, "alice_mask.png")))

            stopwords = set(STOPWORDS)
            stopwords.add("said")

            wc = WordCloud(background_color="white", max_words=2000, mask=alice_mask,
                           stopwords=stopwords)
            # generate word cloud
            wc.generate(text)

            # store to file
            wc.to_file(path.join(d, "alice.png"))

            # show
            plt.imshow(wc, interpolation='bilinear')
            plt.axis("off")
            plt.figure()
            plt.imshow(alice_mask, cmap=plt.cm.gray, interpolation='bilinear')
            plt.axis("off")
            plt.show()

    1. [mask image form here](http://www.stencilry.org/stencils/movies/alice%20in%20wonderland/255fk.jpg)

### fixed

1. python is not installed as a framework

    1. info

            **RuntimeError**: Python is not installed as a framework. The Mac OS X backend will not be able to function correctly if Python is not installed as a framework. See the Python documentation for more information on installing Python as a framework on Mac OS X. Please either reinstall Python as a framework, or try one of the other backends.

    1. fixed by [installation-issue-with-matplotlib-python](https://stackoverflow.com/questions/21784641/installation-issue-with-matplotlib-python)

            # 1. via rc file
            $ cd ~/.matplotlib
            $ nano matplotlibrc
            backend: TkAgg

            # 2. via coding
            import matplotlib as mpl
            mpl.use('TkAgg')
