---
layout: post
title: "dsfs 08 gradient descent"
description: ""
category: [python, data science]
tags: [python, data science, gradient descent]
---
{% include JB/setup %}


### the idea behid gradient descent

1. function

            def sum_of_squares(v):
                """compute the sum of squared elements in v"""
                return sum(v_i ** 2 for v_i in v)

1. estimating the gradient

    1. `f` a function of one variable

            # if `f` if a function of one variable
            # its derivative at a point x measures
            # how f(x) changes when make a very small change to x
            # it is defined as the limit of the difference quotients

            def difference_quotient(f, x, h):
                return (f(x + h) - f(x)) / h

            # as `h` approaches zero

    1. calculate derivative

            def square(x):
                return x * x

            # has the derivative

            def derivative(x):
                return 2 * x

            derivative_estimate = partial(difference_quotient, square, h=0.00001)

            # plot to show they're basically the same
            import matplotlib.pyplot as plt
            x = range(-10, 10)
            plt.title('actual derivative vs. estimate')
            plt.plot(x, map(derivative, x), 'rx', label='actual')
            plt.plot(x, map(derivative_estimate, x), 'b+', label='estimate')
            plt.legend(loc=9)
            plt.show()

    1. `f` function of many variables

            # if `f` is a function of many variables
            # if has multiple `partial derivatives`
            # each indicating how f changes
            # when we make small changes in just one of the input variables

            def partial_difference_quotient(f, v, i, h):
                """compute the ith partial difference quotients of f at v"""
                # add h to just the ith element of v
                w = [v_j + (h if j ==i else 0)
                     for j, v_j in enumerate(v)]

                return (f(w) - f(v)) / h

            # after which we can estimate the gradient the same way

            def estimate_gradient(f, v, h=0.00001):
                return [partial_different_quotient(f, v, i, h)
                        for i, _ in enumerate(v)]

1. using the gradient

            # it's easy to see that
            # the `sum_of_squares` function is smallest
            # when its input `v` is a vector of zeros

            # but imagine we didn't know that
            # let's use gradients to find the minimum

            def step(v, direction, step_size):
                """move step_size in the direction from v"""
                return [v_i + step_size * direction_i
                        for v_i, direction_i in zip(v, direction)]

            def sum_of_squares_gradient(v):
                return [2 * v_i for v_i in v]

            # picl a random starting point
            v = [random.randint(-10, 10) for i in range(3)]

            tolerance = 0.0000001

            while True:
                