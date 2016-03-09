---
layout: post
title: "plot spectogram from mp3"
description: ""
category: [python]
tags: [plot, mp3, ]
---
{% include JB/setup %}


### prerequisites

1. [install libsndfile]()

1. [install scikits.audiolab]({% post_url 2015-04-03-anaconda-on-mac %})

1. [install libav](http://stackoverflow.com/questions/21495863/unable-to-install-avconv-libav)

### install libav

* use homebrew `$ brew install libav`

            $ brew install libav
            ==> Installing dependencies for libav: faac, lame, x264, xvid
            ==> Installing libav dependency: faac
            ==> Downloading https://homebrew.bintray.com/bottles/faac-1.28.yosemite.bottle.1.tar.gz
            ######################################################################## 100.0%
            ==> Pouring faac-1.28.yosemite.bottle.1.tar.gz
            🍺  /usr/local/Cellar/faac/1.28: 13 files, 712K
            ==> Installing libav dependency: lame
            ==> Downloading https://homebrew.bintray.com/bottles/lame-3.99.5.yosemite.bottle.1.tar.gz
            ######################################################################## 100.0%
            ==> Pouring lame-3.99.5.yosemite.bottle.1.tar.gz
            🍺  /usr/local/Cellar/lame/3.99.5: 25 files, 2.1M
            ==> Installing libav dependency: x264
            ==> Downloading https://homebrew.bintray.com/bottles/x264-r2533.yosemite.bottle.tar.gz
            ######################################################################## 100.0%
            ==> Pouring x264-r2533.yosemite.bottle.tar.gz
            🍺  /usr/local/Cellar/x264/r2533: 9 files, 3.4M
            ==> Installing libav dependency: xvid
            ==> Downloading https://homebrew.bintray.com/bottles/xvid-1.3.3.yosemite.bottle.2.tar.gz
            ######################################################################## 100.0%
            ==> Pouring xvid-1.3.3.yosemite.bottle.2.tar.gz
            🍺  /usr/local/Cellar/xvid/1.3.3: 9 files, 1.3M
            ==> Installing libav
            ==> Downloading https://homebrew.bintray.com/bottles/libav-11.3.yosemite.bottle.tar.gz
            ######################################################################## 100.0%
            ==> Pouring libav-11.3.yosemite.bottle.tar.gz
            🍺  /usr/local/Cellar/libav/11.3: 8 files,  18M

### code from stackoverflow

            import os
            from subprocess import check_call
            from tempfile import mktemp
            from scikits.audiolab import wavread, play
            from scipy.signal import remez, lfilter
            from pylab import *

            # convert mp3, read wav
            mp3filename = 'quit.playing.games.mp3'
            wname = mktemp('.wav')
            check_call(['/usr/local/bin/avconv', '-i', mp3filename, wname])
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

### api

1. [scipy.signal.remez](http://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.remez.html)

    * scipy.signal.remez(numtaps, bands, desired, weight=None, Hz=1, type='bandpass', maxiter=25, grid_density=16)[source]

    * [reference](https://gist.github.com/WarrenWeckesser/67bef61f496080aeb798)


1. [scipy.signal.lfilter](http://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.lfilter.html)

    * scipy.signal.lfilter(b, a, x, axis=-1, zi=None)


### todo 1

* there is still some error here

            check_call(['avconv', '-i', mp3filename, wname])

            raise child_exception

            OSError: [Errno 2] No such file or directory

* solution

    * use full path of `avconv`

            $ which avconv
            /usr/local/bin/avconv

    * run `check_call`

            In [4]: from subprocess import check_call
               ...: from tempfile import mktemp
               ...: mp3filename = 'quit.playing.games.mp3'
               ...: wname = mktemp('.wav')
               ...: check_call(['/usr/local/bin/avconv', '-i', mp3filename, wname])
               ...: 

            Out[4]: 0

    * [reference](http://stackoverflow.com/questions/13786797/python-subprocess-call-no-such-file-or-directory)

### todo 2

* problem

            Traceback (most recent call last):

              File "<ipython-input-6-42c328711091>", line 20, in <module>
                sig_filt /=  1.05 * max(abs(sig_filt)) # normalize

            ValueError: The truth value of an array with more than one element is ambiguous. Use a.any() or a.all()

* solution