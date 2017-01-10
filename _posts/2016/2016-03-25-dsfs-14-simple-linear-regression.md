---
layout: post
title: "dsfs 14 simple linear regression"
description: ""
category: [python, data science]
tags: [python, data science, linear regression]
---
{% include JB/setup %}


### the model

1. user's number of friends and amount of time spend on the site each day

            # assume more friends causes people to spend more time

            # in particular
            # you hypothesize that there are constants α and β
            yi = βxi + α + εi

            # where yi is the number of minutes user i spends on the site daily
            # xi is the number of friends user i has
            # εi is a (hopefully small) error term representing the fact
            # that are other factors not accounted for by this simple model

    1. assuming we've determind such alpha and beta

            # then make predictions simply with
            def predict(alpha, beta, x_i):
                return beta * x_i + alpha

            # how to choose alpha and beta
            # since we know the actual output y_i
            # we can compute the error for each pair
            def error(alpha, beta, x_i, y_i):
                """the error from predicting beta * x_i + alpha
                when the actual value is y_i"""
                return y_i - predict(alpha, beta, x_i)

    1. squared errors

            # we don't want to just add the errors
            # the total error over the entir data set

            def sum_of_squared_errors(alpha, beta, x, y):
                return sum(error(alpha, beta, x_i, y_i) ** 2
                           for x_i, y_i in zip(x, y))

    1. the least squares solution

            # is to choose the alpha and beta
            # that make sum_of_squared_errors as small as possible

            # using calculus (or tedious algebra)
            # the error-minimizing alpha and beta are given by
            def least_squares_fit(x, y):
                """given training values for x and y
                find the least-squares values of alpha and beta"""
                beta = correlation(x, y) * standard_deviation(y) / standard_deviation(x)
                alpha = mean(y) - beta * mean(x)
                return alpha, beta