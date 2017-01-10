---
layout: post
title: "python map reduce filter"
description: ""
category: [python]
tags: [map, reduce, filter]
---
{% include JB/setup %}


### map

1. [map(function, iterable, ...)](https://docs.python.org/2/library/functions.html?highlight=map#map)

    1. apply `function` to every item of `iterable` and return a list of the result

    1. if additional iterable arguments are passed, function must take that many arguments and is applied to the items from all iterables in parallel

    1. if one iterable is shorter than another it is assumed to be extended with none items

    1. if function is none, the identity function is assumed

    1. if there are multiple arguments, map() returns a list consisting of tuples containing the corresponding items from all iterables (a kind of transpose operation)

    1. the iterable arguments may be a sequence or any iterable object

    1. the result is always a list


### reduce

1. [reduce(function, iterable[, initializer])](https://docs.python.org/2/library/functions.html?highlight=map#reduce)

    1. apply function of two arguments cumulatively to the items of iterable, from left to right, so as to reduce the iterable to a single value

    1. e.g., reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) calculates ((((1+2)+3)+4)+5)

    1. the left argument, x, is the accumulated value and the right argument, y, is the update value from the iterable

    1. if the optional initializer is present, it is placed before the items of the iterable in the calculation, and serves as a default when the iterable is empty

    1. if initializer is not given and iterable contains only one item, the first item is returned.

1. demos

    1. [make python list unique in functional way map reduce filter](http://stackoverflow.com/questions/13757835/make-python-list-unique-in-functional-way-map-reduce-filter)

            [1, 2, 2, 3, 3, 3, 4] => [1, 2, 3, 4]

            your_list = [1, 2, 2, 3, 3, 3, 4]
            print reduce(lambda x, y: x+[y] if x == [] or x[-1] != y else x, your_list, [])
            print reduce(lambda x, y: x+[y] if y not in x else x, your_list, [])

            [1, 2, 3, 4]
            [1, 2, 3, 4]

            your_list = [1, 3, 3, 2, 2, 3, 3, 3, 4]
            print reduce(lambda x, y: x+[y] if x == [] or x[-1] != y else x, your_list, [])
            print reduce(lambda x, y: x+[y] if y not in x else x, your_list, [])

            [1, 3, 2, 3, 4]
            [1, 3, 2, 4]

### filter

1. [filter(function, iterable)](https://docs.python.org/2/library/functions.html?highlight=map#filter)

    1. construct a list from those elements of iterable for which function returns true

    1. iterable may be either a sequence, a container which supports iteration, or an iterator

    1. if iterable is a string or a tuple, the result also has that type; otherwise it is always a list

    1. if function is none, the identity function is assumed, that is, all elements of iterable that are false are removed.

            note that
            filter(function, iterable) is equivalent to
            [item for item in iterable if function(item)] if function is not none and
            [item for item in iterable if item] if function is none.


### fixed

1. [how to use filter map and reduce in python 3 3 0](http://stackoverflow.com/questions/13638898/how-to-use-filter-map-and-reduce-in-python-3-3-0)

    1. [map()](http://docs.python.org/3.0/library/functions.html#map) and [filter()](http://docs.python.org/3.0/library/functions.html#filter) return iterators

            use list comprehension
            correct transformation is to use a regular `for` loop

            [i for i in range(2, 25) if f(i)]

            [cube(i) for i in range(1, 11)]


    1. reduce() removed use [functools.reduce()](http://docs.python.org/3.0/library/functools.html#functools.reduce)

            explicit use `for` loop
