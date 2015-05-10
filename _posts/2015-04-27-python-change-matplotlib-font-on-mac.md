---
layout: post
title: "python change matplotlib font on mac"
description: ""
category: [python]
tags: [font, matplotlib, mac]
---
{% include JB/setup %}


### solution

* [reference blog.olgabotvinnik.com](http://blog.olgabotvinnik.com/blog/2012/11/15/2012-11-15-how-to-set-helvetica-as-the-default-sans-serif-font-in/)

1. use FontBook app to install msyh.ttf

    * this will install msyh.ttf for `User` use, check my [2015-04-27-mac-font-locations-and-their-purposes]({% post_url 2015-04-27-mac-font-locations-and-their-purposes %})

            $ cd ~/Library/Fonts; ls
            msyh.ttf

    * after install you will find out this font called `Microsoft YaHei`

1. edit your `matplotlibrc` file

    1. find out `matplotlibrc` file

            $ python -c "import matplotlib; print matplotlib.matplotlib_fname()"
            /Users/hqlgree2/anaconda/lib/python2.7/site-packages/matplotlib/mpl-data/matplotlibrc

    1. copy this file to your home folder

            $ cd /Users/hqlgree2/anaconda/lib/python2.7/site-packages/matplotlib/mpl-data/
            $ cp matplotlibrc ~/.matplotlib/matplotlibrc

    1. make some edit and save

            $ pico matplotlibrc
            # uncomment this line
            font.family : sans-serif

            # uncomment this line and add Microsoft YaHei
            font.sans-serif : Microsoft YaHei, Bitstream Vera Sans...

1. regenerate `fontList.cache`

            $ cd ~/.matplotlib
            $ rm fontList.cache

            $ python -c "import matplotlib.pyplot as plt"

            $ grep YaHei fontList.cache 
            S'Microsoft YaHei'

1. use font in code

            import matplotlib
            matplotlib.rcParams['font.sans-serif'] = ['Microsoft YaHei']
            matplotlib.rcParams['axes.unicode_minus'] = False

### convert `.dfont` to `.ttf`

1. install fondu use brew

            $ brew install fondu
            # if got an error run brew update

1. convert

            $ cd some_dir
            $ sudo fondu -show /System/Library/Fonts/Helvetica.dfont

### convert `.ttc` to `.ttf`

1. download [dfontsplitter](http://peter.upfold.org.uk/projects/dfontsplitter)

1. drag `.ttc` file into `dfontsplitter` app

### find font on your system

1. you can use FontBook.app

1. check my [2015-04-27-mac-font-locations-and-their-purposes]({% post_url 2015-04-27-mac-font-locations-and-their-purposes %})

### find where matplotlib stores its data

            $ python
            >>> import matplotlib
            >>> matplotlib.matplotlib_fname()
            /Users/hqlgree2/anaconda/lib/python2.7/site-packages/matplotlib/mpl-data/matplotlibrc
            or

            $ python -c "import matplotlib; print matplotlib.matplotlib_fname()"
            /Users/hqlgree2/anaconda/lib/python2.7/site-packages/matplotlib/mpl-data/matplotlibrc