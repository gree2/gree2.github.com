---
layout: post
title: "python code read wave file and plot"
description: ""
category: [python]
tags: [wave, plot, channel, sampwidth, framerate, nframes, comptype, compname]
---
{% include JB/setup %}


### code

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