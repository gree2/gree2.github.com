---
layout: post
title: "python numpy.random random sampling"
description: ""
category: [python]
tags: [random, rand, randn, randint, random_integers, random_sample, ranf, sample, choice, bytes]
---
{% include JB/setup %}


### simple random data

1. [rand(d0, d1, ..., dn)](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.rand.html#numpy.random.rand)

    * random values in a given shape

            In [2]: np.random.rand(3)
            Out[2]: array([ 0.2978406 ,  0.18461514,  0.24704652])

            In [3]: np.random.rand(3, 5)
            Out[3]: 
            array([[ 0.15870093,  0.49928607,  0.39069335,  0.7445641 ,  0.3208757 ],
                   [ 0.03554006,  0.11208914,  0.4263552 ,  0.19699995,  0.17269604],
                   [ 0.393527  ,  0.33498196,  0.8278391 ,  0.22020756,  0.97791024]])

1. [randn(d0, d1, ..., dn)](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.randn.html#numpy.random.randn)

    * return a sample (or samples) from the "standard normal" ditribution

            In [4]: np.random.randn(3)
            Out[4]: array([-0.3169426 , -0.62385199, -0.3950756 ])

            In [5]: np.random.randn(3, 5)
            Out[5]: 
            array([[ 0.55238932, -1.46249184,  1.83115131, -0.75392455,  0.34590461],
                   [-0.71300575,  0.23139916,  0.42955918, -1.00816456, -0.79046912],
                   [ 0.27605609,  2.53116393, -0.15409915, -0.90058645,  0.43619731]])

1. [randint(low[, high, size])](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.randint.html#numpy.random.randint)

    * return random integers [low, high)

            In [6]: np.random.randint(3)
            Out[6]: 0

            In [7]: np.random.randint(3, 5)
            Out[7]: 4

            In [8]: np.random.randint(3, 9)
            Out[8]: 3

1. [random_integers(low[, high, size])](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.random_integers.html#numpy.random.random_integers)

    * return random integers [low, high]

            In [11]: np.random.random_integers(3)
            Out[11]: 1

            In [12]: np.random.random_integers(3, 9)
            Out[12]: 4

1. [random_sample([size])](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.random_sample.html#numpy.random.random_sample)

    * return random floats in the half-open interval [0.0, 1.0)

            In [13]: np.random.random_sample(3)
            Out[13]: array([ 0.4559776 ,  0.76772504,  0.46774327])

1. [random([size])](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.random.html#numpy.random.random)

    * return random floats in the half-open interval [0.0, 1.0)

            In [15]: np.random.random(3)
            Out[15]: array([ 0.30355838,  0.18042125,  0.3204727 ])

1. [ranf([size])](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.ranf.html#numpy.random.ranf)

    * return random floats in the half-open interval [0.0, 1.0)

            In [16]: np.random.ranf(3)
            Out[16]: array([ 0.75417093,  0.83522699,  0.47386136])

1. [sample(size)](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.sample.html#numpy.random.sample)

    * return random floats in the half-open interval [0.0, 1.0)

            In [17]: np.random.sample(3)
            Out[17]: array([ 0.39996352,  0.89666635,  0.3100504 ])

1. [choice(a[, size, replace, p])](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.choice.html#numpy.random.choice)

    * generates a random sample from a given 1-d array

            In [18]: np.random.choice(3)
            Out[18]: 2

            In [19]: np.random.choice(3, 5)
            Out[19]: array([2, 2, 2, 1, 0])

            In [20]: np.random.choice(3, 5, 2)
            Out[20]: array([2, 2, 0, 2, 2])

1. [bytes(length)](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.bytes.html#numpy.random.bytes)

    * return random bytes

            In [27]: np.random.bytes(3)
            Out[27]: '\xd1\x1a\xec'

            In [28]: np.random.bytes(5)
            Out[28]: '8\x10\x1e\x96\xc1'

### permutations

1. [shuffle(x)](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.shuffle.html#numpy.random.shuffle)

    * modify a sequence in-place by shuffling its contents

            In [33]: a = [1, 2, 3, 4, 5, 6, 7, 8, 9]

            In [34]: a
            Out[34]: [1, 2, 3, 4, 5, 6, 7, 8, 9]

            In [35]: np.random.shuffle(a)

            In [36]: a
            Out[36]: [6, 5, 4, 2, 3, 8, 7, 1, 9]

1. [permutation(x)](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.permutation.html#numpy.random.permutation)

    * randomly permute a sequence, or return a permuted range

            In [37]: a = [1, 2, 3, 4, 5, 6, 7, 8, 9]

            In [38]: np.random.permutation(a)
            Out[38]: array([7, 6, 2, 5, 8, 3, 9, 1, 4])

### distributions

1. todo