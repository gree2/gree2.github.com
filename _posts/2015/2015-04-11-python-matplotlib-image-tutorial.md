---
layout: post
title: "python matplotlib image tutorial"
description: ""
category: [python]
tags: [matplotlib, image]
---
{% include JB/setup %}


### startup command

            $ ipython
            In [1]: import matplotlib.pyplot as plt
            In [2]: import matplotlib.image as mpimg
            In [3]: import numpy as np

* you can now access functions like `imshow()` by using:

            plt.imshow(yourimage)

### importing image data into numpy arrays

* plotting image data is supported by the [Pillow](http://python-imaging.github.io/)

* matplotlib only supports `PNG` images

* the commands shown below fall back on pillow if the native read fails

### image going to play with

![stinkbug](http://matplotlib.org/_images/stinkbug.png)

* it's a 24-bit RGB png image (8 bits for each of r, g, b)

* other kinds of image that you'll most likely encounter are RGBA images

    * allow for transparency

    * single channel grayscale (luminosity) images

### read image from local file

* read image file

    * tutorial 1

            In [4]: img =  mpimg.imread('stinkbug.png')
            array([[[ 0.40784314,  0.40784314,  0.40784314],
                    [ 0.40784314,  0.40784314,  0.40784314],
                    [ 0.40784314,  0.40784314,  0.40784314],
                    ..., 
                    [ 0.42745098,  0.42745098,  0.42745098],
                    [ 0.42745098,  0.42745098,  0.42745098],
                    [ 0.42745098,  0.42745098,  0.42745098]],

                   [[ 0.41176471,  0.41176471,  0.41176471],
                    [ 0.41176471,  0.41176471,  0.41176471],
                    [ 0.41176471,  0.41176471,  0.41176471],
                    ..., 
                    [ 0.42745098,  0.42745098,  0.42745098],
                    [ 0.42745098,  0.42745098,  0.42745098],
                    [ 0.42745098,  0.42745098,  0.42745098]],

                   [[ 0.41960785,  0.41960785,  0.41960785],
                    [ 0.41568628,  0.41568628,  0.41568628],
                    [ 0.41568628,  0.41568628,  0.41568628],
                    ..., 
                    [ 0.43137255,  0.43137255,  0.43137255],
                    [ 0.43137255,  0.43137255,  0.43137255],
                    [ 0.43137255,  0.43137255,  0.43137255]],

                   ..., 
                   [[ 0.43921569,  0.43921569,  0.43921569],
                    [ 0.43529412,  0.43529412,  0.43529412],
                    [ 0.43137255,  0.43137255,  0.43137255],
                    ..., 
                    [ 0.45490196,  0.45490196,  0.45490196],
                    [ 0.4509804 ,  0.4509804 ,  0.4509804 ],
                    [ 0.4509804 ,  0.4509804 ,  0.4509804 ]],

                   [[ 0.44313726,  0.44313726,  0.44313726],
                    [ 0.44313726,  0.44313726,  0.44313726],
                    [ 0.43921569,  0.43921569,  0.43921569],
                    ..., 
                    [ 0.4509804 ,  0.4509804 ,  0.4509804 ],
                    [ 0.44705883,  0.44705883,  0.44705883],
                    [ 0.44705883,  0.44705883,  0.44705883]],

                   [[ 0.44313726,  0.44313726,  0.44313726],
                    [ 0.4509804 ,  0.4509804 ,  0.4509804 ],
                    [ 0.4509804 ,  0.4509804 ,  0.4509804 ],
                    ..., 
                    [ 0.44705883,  0.44705883,  0.44705883],
                    [ 0.44705883,  0.44705883,  0.44705883],
                    [ 0.44313726,  0.44313726,  0.44313726]]], dtype=float32)


    * code 1

            import matplotlib.pyplot as plt
            import matplotlib.image as mpimg
            import numpy as np
            img = mpimg.imread('stinkbug.png')
            imgplot = plt.imshow(img)

* dtype

    * `img`'s dtype is float32

    * matplotlib has rescaled the 8 bits data from each channel to floating point data between 0.0 and 1.0

    * the only datatype that pillow can work with is `uint8`

    * matplotlib plotting can handle `float32` and `uint8`

    * but image reading/writing for any format other than PNG is limited to `uint8` data

* list

    * each inner list represents a pixel

    * with an rgb image there are 3 values

    * since it's a black and white image, r, g, and b are all similar

    * an RGBA (where A is alpha, or transparency) has 4 values per inner list

    * and a simple luminanace image just has one value (and is thus only a 2-d array, not a 3-d array)

    * for `RGB` and `RGBA` images matplotlib supports `float32` and `uint8` data type

    * for grayscale matplotlib supports only `float32`

### plotting numpy arrays as images

* so you have your data in a numpy array

    * importing it

    * generating it

            In [5]: imgplt = plt.imshow(img)

    ![image_tutorial-1](http://matplotlib.org/_images/image_tutorial-1.png)

* you can also plot any numpy array

    * just remember that the datatype must be `float32`

            range from 0.0 to 1.0

    * or `uint8`

### applying pseudocolor schemes to image plots

* pseudocolor can be a useful tool for `enhancing contrast` and `visualizing` your data more easily

* pseudocolor is only relevant to

    * single-channel images

    * grayscale images

    * luminosity images

* pick one channel of our data

    * tutorial 2

            In [6]: lum_img = img[:,:,0]

            In [7]: imgplot = plt.imshow(lum_img)

    ![image_tutorial-2](http://matplotlib.org/_images/image_tutorial-2.png)

    * code 2

            import matplotlib.pyplot as plt
            import matplotlib.image as mpimg
            import numpy as np
            img = mpimg.imread('stinkbug.png')
            lum_img = img[:,:,0]
            plt.imshow(lum_img)

* with a luminosity image, the default colormap (aka lookup table, LUT) is applied

* the default is called `jet`

* you can set some others using the `set_cmap()` method on our image plot object

    * tutorial 3

            In [8]: imgplot.set_cmap('hot')

    ![image_tutorial-3](http://matplotlib.org/_images/image_tutorial-3.png)

    * code 3

            import matplotlib.pyplot as plt
            import matplotlib.image as mpimg
            import numpy as np
            img = mpimg.imread('../_static/stinkbug.png')
            lum_img = img[:,:,0]
            imgplot = plt.imshow(lum_img)
            imgplot.set_cmap('hot')

    * tutorial 4

            In [9]: imgplot.set_cmap('spectral')

    ![image_tutorial-4](http://matplotlib.org/_images/image_tutorial-4.png)

    * code 4

            import matplotlib.pyplot as plt
            import matplotlib.image as mpimg
            import numpy as np
            img = mpimg.imread('stinkbug.png')
            lum_img = img[:,:,0]
            imgplot = plt.imshow(lum_img)
            imgplot.set_cmap('spectral')

### color scale reference

* it's helpful to have an idea of what value a color represents

* we can do that by adding color bars

    * tutorial 5

            In [10]: plt.colorbar()

    ![image_tutorial-5](http://matplotlib.org/_images/image_tutorial-5.png)

    * code 5

            import matplotlib.pyplot as plt
            import matplotlib.image as mpimg
            import numpy as np
            img = mpimg.imread('stinkbug.png')
            lum_img = img[:,:,0]
            imgplot = plt.imshow(lum_img)
            imgplot.set_cmap('spectral')
            plt.colorbar()

### examining a specific data range

* sometimes you want to enhance the contrast in your image

* or expand the contrast in a particular region while sacrificing the detail in colors that don't vary much

* a good tool to find interesting regions is the histogram use `hist()` function

    * tutorial 6

            In [11]: plt.hist(lum_img.flatten(), 256, range=(0.0, 1.0), fc='k', ec='k')

    ![image_tutorial-6](http://matplotlib.org/_images/image_tutorial-6.png)

    * code 6

            import matplotlib.pyplot as plt
            import matplotlib.image as mpimg
            import numpy as np
            img = mpimg.imread('stinkbug.png')
            lum_img = img[:,:,0]
            plt.hist(lum_img.flatten(), 256, range=(0.0,1.0), fc='black', ec='black')

    * tutorial 7

            In [12]: imgplot.set_clim(0.0, 0.7)

    ![image_tutorial-7](http://matplotlib.org/_images/image_tutorial-7.png)

    * code 7

            import matplotlib.pyplot as plt
            import matplotlib.image as mpimg
            import numpy as np
            fig = plt.figure()
            a=fig.add_subplot(1,2,1)
            img = mpimg.imread('stinkbug.png')
            lum_img = img[:,:,0]
            imgplot = plt.imshow(lum_img)
            a.set_title('Before')
            plt.colorbar(ticks=[0.1,0.3,0.5,0.7], orientation ='horizontal')
            a=fig.add_subplot(1,2,2)
            imgplot = plt.imshow(lum_img)
            imgplot.set_clim(0.0,0.7)
            a.set_title('After')
            plt.colorbar(ticks=[0.1,0.3,0.5,0.7], orientation='horizontal')

### array interpolation schemes

* interpolation calculates what the color or value of a pixel `should` be

* according to different mathematical schemes

    * tutorial 8

            In [13]: from PIL import Image
            # open image as pillow image object
            In [14]: img = Image.open('stinkbug.png')
            # use pillow to resize
            In [15]: rsize = img.resize((img.size[0]/10, img.size[1]/10))
            # get array back
            In [16]: rsizeArr = np.asarray(rsize)
            In [17]: imgplot = plt.imshow(rsizeArr)

    ![image_tutorial-8](http://matplotlib.org/_images/image_tutorial-8.png)

    * code 8

            import matplotlib.pyplot as plt
            import matplotlib.image as mpimg
            import numpy as np
            from PIL import Image
            # opens the file using Pillow - it's not an array yet
            img = Image.open('../_static/stinkbug.png')
            # resize the image
            rsize = img.resize((img.size[0]/10,img.size[1]/10))
            rsizeArr = np.asarray(rsize)
            lum_img = rsizeArr[:,:,0]
            imgplot = plt.imshow(rsizeArr)

    * tutorial 9

            In [18]: imgplot.set_interpolation('nearest')

    ![image_tutorial-9](http://matplotlib.org/_images/image_tutorial-9.png)

    * code 9

            import matplotlib.pyplot as plt
            import matplotlib.image as mpimg
            import numpy as np
            from PIL import Image
            # opens the file using Pillow - it's not an array yet
            img = Image.open('../_static/stinkbug.png')
            # resize the image
            rsize = img.resize((img.size[0]/10,img.size[1]/10))
            rsizeArr = np.asarray(rsize)
            lum_img = rsizeArr[:,:,0]
            imgplot = plt.imshow(rsizeArr)
            imgplot.set_interpolation('nearest')

    * tutorial 10

            In [19]: imgplot.set_interpolation('bicubic')

    ![image_tutorial-10](http://matplotlib.org/_images/image_tutorial-10.png)

    * code 10

            import matplotlib.pyplot as plt
            import matplotlib.image as mpimg
            import numpy as np
            from PIL import Image
            # opens the file using Pillow - it's not an array yet
            img = Image.open('../_static/stinkbug.png')
            # resize the image
            rsize = img.resize((img.size[0]/10,img.size[1]/10))
            rsizeArr = np.asarray(rsize)
            lum_img = rsizeArr[:,:,0]
            imgplot = plt.imshow(rsizeArr)
            imgplot.set_interpolation('bicubic')

    * bicubic interpolation is often used when blowing up photos - people tend to prefer blurry over pixelated

### reference

[matplotlib image tutorial](http://matplotlib.org/users/image_tutorial.html)
