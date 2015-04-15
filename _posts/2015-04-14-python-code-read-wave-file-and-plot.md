---
layout: post
title: "python code read wave file and plot"
description: ""
category: [python]
tags: [wave, plot, channel, sampwidth, framerate, nframes, comptype, compname]
---
{% include JB/setup %}


### origin code

1. [reference](http://sebug.net/paper/books/scipydoc/wave_pyaudio.html)

                import wave
                import pylab as pl
                import numpy as np

                f = wave.open(r"c:\\WINDOWS\\Media\\ding.wav", "rb")

                # (nchannels, sampwidth, framerate, nframes, comptype, compname)
                params = f.getparams()
                nchannels, sampwidth, framerate, nframes = params[:4]

                str_data = f.readframes(nframes)
                f.close()

                wave_data = np.fromstring(str_data, dtype=np.short)
                wave_data.shape = -1, 2
                wave_data = wave_data.T
                time = np.arange(0, nframes) * (1.0 / framerate)

                pl.subplot(211) 
                pl.plot(time, wave_data[0])
                pl.subplot(212) 
                pl.plot(time, wave_data[1], c="g")
                pl.xlabel("time (seconds)")
                pl.show()

    ![wave_pyaudio_01](http://sebug.net/paper/books/scipydoc/_images/wave_pyaudio_01.png)

### modified code

* code

                import wave
                import matplotlib.pyplot as plt
                import numpy as np

                f = wave.open("quit.playing.games.wav", "rb")

                # (nchannels, sampwidth, framerate, nframes, comptype, compname)
                params = f.getparams()
                nchannels, sampwidth, framerate, nframes = params[:4]

                str_data = f.readframes(nframes)
                f.close()

                wave_data = np.fromstring(str_data, dtype=np.short)
                wave_data.shape = -1, 2
                wave_data = wave_data.T
                time = np.arange(0, nframes) * (1.0 / framerate)

                figure = plt.gcf() # get current figure
                figure.set_size_inches(8*20, 6)

                duration = nframes/float(framerate)
                xticks = np.arange(0, duration, 2)
                plt.subplot(211).set_xticks(xticks)
                plt.plot(time, wave_data[0])
                plt.title('quit.playing.games.mp3 channel 1', loc='left')

                plt.subplot(212).set_xticks(xticks)
                plt.plot(time, wave_data[1], c="g")
                plt.xlabel("time (seconds)")
                plt.title('quit.playing.games.mp3 channel 2', loc='left')
                plt.savefig('quit.playing.games.png', dpi=100, bbox_inches='tight', pad_inches=0.1)
                #plt.show()
                plt.close(figure)

    [including images and resources](http://jekyllrb.com/docs/posts/#including-images-and-resources)

    ![quit.playing.games](/assets/post/quit.playing.games.png)

### api

1. [matplotlib.figure.Figure.set_size_inches(w, h, forword=False)](http://matplotlib.org/api/figure_api.html#matplotlib.figure.Figure.set_size_inches)

    * set the figure size in inches (1in == 2.54cm)

    * usage:

                fig.set_size_inches(w, h) # or
                fig.set_size_inches((w, h))

    * optional kwarg `forword=Ture` will cause the canvas size to be automatically updated

1. [matplotlib.figure.Figure.savefig(*args, **kwargs)](http://matplotlib.org/api/figure_api.html#matplotlib.figure.Figure.savefig)

    * save the current figure

    * call signature

                savefig(fname, dpi=None, facecolor='w', edgecolor='w',
                        orientation='portrait', papertype=None, format=None,
                        transparent=False, bbox_inches=None, pad_inches=0.1, frameon=None)

    1. arguments

        1. fname

                a string containing a path to a filename
                if format is None and fname is a string
                the output format is deduced from the extension of the filename

    1. keyword arguments

        1. dpi

                [None | scalar > 0]
                the resolution in dots per inche

        1. facecolor, edgecolor

                the colors of the figure rectangle

        1. orientation ['landscape', 'portrait']

                not supported on all backends
                current only on postscript output

        1. papertype

                one of 'letter', 'legal', 'executive', 'ledger'
                'a0' through 'a10'
                'b0' through 'b10'
                only supported for postscript output

        1. format

                one of file extensions supported by the active backend
                most backends support png, pdf, ps, eps and svg

        1. transparent

                if True the axes patches will all be transparent
                the figure patch will also be transparent unless facecolor and/or edgecolor are specified via kwargs

        1. frameon

                if True the figure patch will be colored
                if False the figure background will be transparent

        1. bbox_inches

                Bbox in inches
                only the given portion of the figure is saved
                if 'tight' try to figure out the tight bbox of the figure

        1. pad_inches

                amount of padding around the figure when bbox_inches is 'tight'

        1. bbox_extra_artists

                a list of extra artists that will be considered when the tight bbox is calculated

### reference

1. [get wav file duration](http://stackoverflow.com/questions/7833807/get-wav-file-length-or-duration)

    * [python package songdetails]({% post_url 2015-04-04-python-package-songdetails %})

                In [16]: >>> import songdetails
                    ...: >>> song = songdetails.scan("quit.playing.games.mp3")
                    ...: >>> song.duration
                    ...: >>> print song.duration
                    ...: 
                0:03:58

                In [17]: 3 * 60 + 58
                238

    * `duration = nframes/float(framerate)`

                the duration is equal to the number of frames divided by the framerate (frames per second)

                In [18]: duration = nframes/float(framerate)

                In [19]: duration
                Out[19]: 238.00163265306122
