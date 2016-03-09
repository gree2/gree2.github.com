---
layout: post
title: "python package songdetails"
description: ""
category: [python]
tags: [mp3, package, pure]
---
{% include JB/setup %}

### info

1. pure python package for retrieving detatils of songs

1. dependencies

    1. [mpeg1audio](http://github.com/Ciantic/mpeg1audio/)

    1. [pytagger](http://github.com/Ciantic/pytagger/)

### installation

1. install mpeg1audio

    * download [mpeg1audio](http://github.com/Ciantic/mpeg1audio/)

    * unzip mpeg1audio and install

            $ cd mpeg1audiod-master
            $ ./setup.py install

1. install pytagger

    * download [pytagger](http://github.com/Ciantic/pytagger/)

    * unzip pytagger and install

            $ cd pytagger-master
            $ ./setup.py install

1. install songdetails

    * download [songdetails](https://github.com/Ciantic/songdetails)

    * unzip songdetails and install

            $ cd songdetails-master
            $ ./setup.py install

### example

    >>> import songdetails
    >>> song = songdetails.scan("quit.playing.games.mp3")
    >>> song.duration
    datetime.timedelta(0, 239)
    >>> print song.duration
    0:03:59