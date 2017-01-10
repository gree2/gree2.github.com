---
layout: post
title: "python functools partial"
description: ""
category: [python]
tags: [functools, partial]
---
{% include JB/setup %}


### [partial](https://docs.python.org/2/library/functools.html#functools.partial)

1. demo

            >>> from functools import partial
            >>> basetwo = partial(int, base=2)
            >>> basetwo.__doc__ = 'convert base 2 string to an int'
            >>> basetwo('10010')
            18

### [demo](http://www.pydanny.com/python-partials-are-fun.html)

1. make new version of a function with one or more arguments already filled in

1. new version of a function documents itself

1. demo exponentiation

    1. power

            def power(base, exponent):
                return base ** exponent

    1. square

            def square(base):
                return power(base, 2)

    1. cube

            def cube(base):
                return power(base, 3)

    1. use partial

            from functools import partial

            square = partial(power, exponent=2)
            cube = partial(power, exponent=3)

            def test_partial():
                assert square(2) == 4
                assert cube(2) == 8

            def test_partial_docs():
                assert square.keywords == {'exponent': 2}
                assert square.func == power

            def test_power_partials():
                power_partials = []
                for x in range(1, 11):
                    f = partial(power, exponent=x)
                    power_partials.append(f)
                # use list comprehension instead of loop
                [partial(power, exponent=x) for x in range(1, 11)]

                assert power_partials[0][2] == 2
                assert power_partials[4][2] == 32
                assert power_partials[9][2] == 1024
