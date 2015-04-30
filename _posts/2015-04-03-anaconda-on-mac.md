---
layout: post
title: "anaconda on mac"
description: ""
category: [python]
tags: [anaconda, python mac]
---
{% include JB/setup %}

### installation

1. installer

    * double click `.pkg` file

    * follow the instructions on the screen


2. command-line installs

            $ bash Anaconda-2.2.0-MacOSX-x86_64.sh

### uninstalling

1. you simply remove the entire install location directory

### updating from older anaconda versions

            $ conda update conda
            $ conda update anaconda

### install scikits-audiolab from source

1. you must install [libsndfile]({% post_url 2015-04-12-install-libsndfile-on-mac %}) first

1. download from [pypi](https://pypi.python.org/pypi/scikits.audiolab/)

1. unpack

1. `$ python setup.py install`

1. [example](http://stackoverflow.com/questions/15311853/plot-spectogram-from-mp3)

            import os
            from subprocess import check_call
            from tempfile import mktemp
            from scikits.audiolab import wavread, play
            from scipy.signal import remez, lfilter
            from pylab import *

            # convert mp3, read wav
            mp3filename = 'XC124158.mp3'
            wname = mktemp('.wav')
            check_call(['avconv', '-i', mp3filename, wname])
            sig, fs, enc = wavread(wname)
            os.unlink(wname)

            # bandpass filter
            bands = array([0,3500,4000,5500,6000,fs/2.0]) / fs
            desired = [0, 1, 0]
            b = remez(513, bands, desired)
            sig_filt = lfilter(b, 1, sig)
            sig_filt /=  1.05 * max(abs(sig_filt)) # normalize

            subplot(211)
            specgram(sig, Fs=fs, NFFT=1024, noverlap=0)
            axis('tight'); axis(ymax=8000)
            title('Original')
            subplot(212)
            specgram(sig_filt, Fs=fs, NFFT=1024, noverlap=0)
            axis('tight'); axis(ymax=8000)
            title('Filtered')
            show()

            play(sig_filt, fs)

### conda and binstar

1. use `conda` to install a package

            $ conda install cjklib
            Fetching package metadata: ....
            Error: No packages found in current osx-64 channels matching: cjklib

            You can search for this package on Binstar with

                binstar search -t conda cjklib

1. then use `binstar`

            $ binstar search -t conda cjklib
            Using binstar api site https://api.binstar.org
            Run 'binstar show <USER/PACKAGE>' to get more details:
            Packages:
                 Name                      |  Version | Package Types   | Platforms      
                 ------------------------- |   ------ | --------------- | ---------------
                 auto/cjklib               |    0.3.2 | conda           | linux-64, linux-32
                                                      : http://cjklib.org
                 travis/cjklib             |    0.3.2 | conda           | linux-64       
                                                      : http://cjklib.org
            Found 2 packages

1. use `binstar show` to get more details

            $ binstar show travis/cjklib
            Using binstar api site https://api.binstar.org
            Name:    cjklib
            Summary: http://cjklib.org
            Access:  public
            Package Types:  conda
            Versions:
               + 0.3.2

            To install this package with conda run:
                 conda install --channel https://conda.binstar.org/travis cjklib

1. install package

            $ conda install --channel https://conda.binstar.org/travis cjklib
            Fetching package metadata: ......
            Error: No packages found in current osx-64 channels matching: cjklib

            You can search for this package on Binstar with

                binstar search -t conda cjklib