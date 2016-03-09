---
layout: post
title: "python numpy and ipython scipy2013 part1 the numpy array object"
description: ""
category: [python]
tags: [numpy, scipy, prime]
---
{% include JB/setup %}


### Section contents

* what are numpy and numpy arrays?
* reference documentation
* import conventions
* creating arrays
* functions for creating arrays
* basic data types
* basic visualization
* indexing and slicing
* copies and views
* fancy indexing

start from [1:16:00](https://www.youtube.com/watch?v=UWmZAAfXds4)

### what are numpy and numpy arrays?

* `python` object

    * high-level number object: integers, floating point

    * containers: lists (costless insertion and append), dictionaries (fast lookup)

* `numpy` provides

    * extention package to python for multi-dimensional arrays

    * closer to hardware (efficiency)

    * designed for scientific computation (convenience)

    * also known as array oriented computing

            In [1]: import numpy as np
                    a = np.array([0, 1, 2, 3])
            Out[1]: array([0, 1, 2, 3])

for example, an array containing
    
* values of an experiment/simulation at discrete time steps

* signal recorded by measurement device, e.g. sound wave

* pixels of an image, grey-level or colour

* 3-d data measured at different x-y-z position, e.g. mri scan

* ...

        In [10]: L = range(1000)
        In [11]: %timeit [i**2 for i in L]
                 10000 loops, best of 3: 82.5 µs per loop
        In [12]: a = np.arange(1000)
        In [13]: %timeit a**2
                 100000 loops, best of 3: 2.4 µs per loop

### reference documentation

* on the [web](http://docs.scipy.org/)

* interactive help

        In [14]: np.array?

* looking for something

        In [15]: np.lookfor("create array")
                 Search results for 'create array'
                 ---------------------------------
                 numpy.array
                     Create an array.
                 numpy.memmap
                     Create a memory-map to an array stored in a *binary* file on disk.
                 numpy.diagflat
                     Create a two-dimensional array with the flattened input as a diagonal.

### import conventions

`import numpy as np`

### creating arrays

* 1-d

        In [17]: a = np.array([0, 1, 2, 3])
                 a
        Out[17]: array([0, 1, 2, 3]) 
        In [18]: a.ndim
        Out[18]: 1 
        In [19]: a.shape
        Out[19]: (4L,) 
        In [20]: len(a)
        Out[20]: 4

* 2-d, 3-d

        In [21]: b = np.array([[0, 1, 2], [3, 4, 5]])    # 2 x 3 array
                 b
        Out[21]: array([[0, 1, 2],
                        [3, 4, 5]])
        In [22]: b.ndim
        Out[22]: 2
        In [23]: b.shape
        Out[23]: (2L, 3L)
        In [24]: len(b)     # returns the size of the first dimension
        Out[24]: 2
        In [25]: c = np.array([[[1], [2]], [[3], [4]]])
                 c
        Out[25]: array([[[1],
                         [2]],

                        [[3],
                         [4]]])
        In [26]: c.shape
        Out[26]: (2L, 2L, 1L)

### functions for creating arrays

1. evenly spaced

        a = np.arange(10)      # 0 .. n-1

        b = np.arange(1, 9, 2) # start, end (exclusive), step

1. by number of points

        c = np.linspace(0, 1, 6) # start, end, num-points

        d = np.linspace(0, 1, 5, endpoint=False)

1. common arrays

    * ones

            a = np.ones((3, 3))
            array([[ 1.,  1.,  1.],
                   [ 1.,  1.,  1.],
                   [ 1.,  1.,  1.]])

    * zeros

            b = np.zeros((2, 2))
            array([[ 0.,  0.],
                   [ 0.,  0.]])

    * eye

            c = np.eye(3)
            array([[ 1.,  0.,  0.],
                   [ 0.,  1.,  0.],
                   [ 0.,  0.,  1.]])

    * diag

            d = np.diag(np.array([1, 2, 3, 4]))
            array([[1, 0, 0, 0],
                   [0, 2, 0, 0],
                   [0, 0, 3, 0],
                   [0, 0, 0, 4]])

    * random

            a = np.random.rand(4)  # uniform in [0, 1]
            array([ 0.94078177,  0.12140966,  0.82375129,  0.24835093])

            b = np.random.randn(4) # gaussian
            array([-2.50582905, -1.46824029, -0.15590371, -0.98251245])

            np.random.seed(1234)   # setting the random seed

### basic data types

1. int32

        a = np.array([1, 2, 3])
        a.dtype
        dtype('int32')

1. float64 `default` data type

        b = np.array([1., 2., 3.,])
        b.dtype
        dtype('float64')

        c = np.array([1, 2, 3], dtype=float)
        c.dtype
        dtype('float64')


1. complex

        d = np.array([1+2j, 3+4j, 5+6*1j])
        d.dtype
        dtype('complex128')

1. bool

        e = np.array([True, False, False, True])
        e.dtype
        dtype('bool')

1. string

        f = np.array(['bonjour', 'hello', 'hallo'])
        f.dtype
        dtype('S7')

### basic visualization

1. start ipython

    * start by launching ipython in pylab mode

            $ ipython --pylab

    * or the notebook

            $ ipython notebook --pylab=inline

1. ipython started

    * ipython

            In [1]: %pylab

    * from the notebook

            In [1]: %pylab inline

1. matplotlab is a 2d plotting package

            import matplotlab.pyplot as plt # the tidy way

            plt.plot(x, y) # line plot
            plt.show()     # shows the plot (not needed with pylab)

            # if you are using pylab
            plot(x, y) # line plot

    * using `import matplotlab.pyplot as plt` is recommended for use in scripts

    * `pylab` is recommended for interactive exploratory work

1. 1d plotting

            x = np.linspace(0, 3, 20)
            y = np.linspace(0, 9, 20)
            plt.plot(x, y)      # line plot

            plt.plot(x, y, 'o') # dot plot

1. 2d arrays (such as images)

            image = np.random.rand(30, 30)
            plt.imshow(image, cmap=plt.cm.hot)
            plt.colorbar()

### indexing and slicing

1. like python sequences (lists)

            a = np.arange(10)
            array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

            a[0], a[2], a[-1]
            (0, 2, 9)

1. reversing a sequence

            a[::-1]
            array([9, 8, 7, 6, 5, 4, 3, 2, 1, 0])

1. multi-dimensional arrays, indexes are tuples of integers

            a = np.diag(np.arange(3))
            array([[0, 0, 0],
                   [0, 1, 0],
                   [0, 0, 2]])

            a[1, 1]
            1

            a[2, 1] = 10 # third line, second column
            array([[0, 0, 0],
                   [0, 1, 0],
                   [0, 10, 2]])

            a[1]
            array([0, 1, 0])

1. slicing arrays

            a = np.arange(10)
            array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

            a[2:9:3]         # [start:end:step]
            array([2, 5, 8])

            a[:4]               # last index is not included
            array([0, 1, 2, 3])

    * all 3 slice components are not required, `start` is 0 `end` is the last `step` is 1

            a[1:3]
            array([1, 2])

            a[::2]
            array([0, 2, 4, 6, 8])

            a[3:]
            array([3, 4, 5, 6, 7, 8, 9])

    * combine assignment and slicing

            a = np.arange(10)
            a[5:] = 10
            array([ 0,  1,  2,  3,  4, 10, 10, 10, 10, 10])

            b = np.arange(5)
            b[5:] = b[::-1]
            array([0, 1, 2, 3, 4, 4, 3, 2, 1, 0])

            np.arange(6)
            array([0, 1, 2, 3, 4, 5])

            np.arange(0, 51, 10)[:, np.newaxis]
            array([[ 0],
                   [10],
                   [20],
                   [30],
                   [40],
                   [50]])

            np.arange(6) + np.arange(0, 51, 10)[:, np.newaxis]
            array([[ 0,  1,  2,  3,  4,  5],
                   [10, 11, 12, 13, 14, 15],
                   [20, 21, 22, 23, 24, 25],
                   [30, 31, 32, 33, 34, 35],
                   [40, 41, 42, 43, 44, 45],
                   [50, 51, 52, 53, 54, 55]])

### copies and views

* a slicing operation creates a `view` on the original array

* which is just a way of accessing array data

* thus the original array is not copied in memory

* you can use `np.may_share_memory()` to check if two arrays share the same memory block

* `when modifying the view, the original array is modified as well`

            a = np.arange(10)
            array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

            b = a[::2]
            array([0, 2, 4, 6, 8])

            np.may_share_memory(a, b)
            Ture

            b[0] = 12
            b
            array([12,  2,  4,  6,  8])
            a
            array([12,  1,  2,  3,  4,  5,  6,  7,  8,  9])

            a = np.arange(10)
            c = a[::2].copy() # force a copy
            c[0] = 12
            a
            array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
            np.may_share_memory(a, b)
            False

* prime number sieve

    1. construct a shape (100,) boolean array `is_prime` and filled with `True`

            is_prime = np.ones((100,), dtype=bool)
            
    1. cross out 0 and 1 which are not primes

            is_prime[:2] = False

    1. for each integer j starting from `2` cross out its higher multiples

            N_max = int(np.sqrt(len(is_prime)))
            for j in range(2, N_max):
                is_prime[2*j::j] = False

    1. `np.nonzero`

            np.nonzero(is_prime)
            (array([ 2,  3,  5,  7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59,
        61, 67, 71, 73, 79, 83, 89, 97], dtype=int64),)

    * all together

            import numpy as np
            is_prime = np.ones((100,), dtype=bool)
            is_prime[:2] = False
            N_max = int(np.sqrt(len(is_prime)))
            for j in range(2, N_max):
                is_prime[2*j::j] = False
            np.nonzero(is_prime)

### fancy indexing

* using boolean masks

            np.random.seed(3)
            a = np.random.random_integers(0, 20, 15)
            a
            array([10,  3,  8,  0, 19, 10, 11,  9, 10,  6,  0, 20, 12,  7, 14])

            (a % 3 == 0)
            array([False,  True, False,  True, False, False, False,  True, False,
                    True,  True, False,  True, False, False], dtype=bool)

            mask = (a % 3 == 0)
            extract_from_a = a[mask] # or a[a%3==0]
            extract_from_a
            array([ 3,  0,  9,  6,  0, 12])

            a[a%3==0] = -1
            array([10, -1,  8, -1, 19, 10, 11, -1, 10, -1, -1, 20, -1,  7, 14])

* indexing with an array of integers

            a = np.arange(0, 100, 10)
            array([ 0, 10, 20, 30, 40, 50, 60, 70, 80, 90])

    * indexing can be done with an array of integers

            a[[2, 3, 2, 4, 2]] # note [2, 3, 2, 4, 2] is a python list
            array([20, 30, 20, 40, 20])

    * new values can be asigned with this kind of indexing

            a[[9, 7]] = -100
            array([   0,   10,   20,   30,   40,   50,   60, -100,   80, -100])

    * when a new array is created by indexing with an array of integer

            # the new array has the same shape than the array of integers
            a = np.arange(10)
            idx = np.array([[3, 4], [9, 7]])
            idx.shape
            (2L, 2L)

            a[idx]
            array([[3, 4],
                   [9, 7]])