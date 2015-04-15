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

            xticks = np.arange(0, 240, 2)
            plt.subplot(211).set_xticks(xticks)
            plt.plot(time, wave_data[0])
            plt.title('quit.playing.games.mp3 channel 1', loc='left')

            plt.subplot(212).set_xticks(xticks)
            plt.plot(time, wave_data[1], c="g")
            plt.xlabel("time (seconds)")
            plt.title('quit.playing.games.mp3 channel 2', loc='left')
            plt.savefig('quit.playing.games.png', dpi=100)
            #plt.show()
            plt.close(figure)

    [including images and resources](http://jekyllrb.com/docs/posts/#including-images-and-resources)

    ![quit.playing.games](/assets/post/quit.playing.games.png)