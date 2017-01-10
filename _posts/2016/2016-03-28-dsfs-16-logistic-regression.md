---
layout: post
title: "dsfs 16 logistic regression"
description: ""
category: [python, data science]
tags: [python, data science, logistic regression]
---
{% include JB/setup %}


### logistic regression

1. the problem

    1. categorical variables

            # 0 no premium account
            # 1 prenium account

    1. data [experience, salary, paid_account]

            # each element is [1, experience, salary]
            x = [[1] + row[:2] for row in data]

            # each element is paid_account
            y = [row[2] for row in data]

    1. use linear regression and find the best model

            paid account = β0 + β1experience + β2salary + ε

            rescaled_x = rescale(x)
            # [0.26, 0.43, -0.43]
            beta = estimate_beta(rescaled_x, y)
            predictions = [predict(x_i, beta) for x_i in rescaled_x]

            plt.scatter(predictions, y)
            plt.xlabel('predicted')
            plt.ylabel('actual')
            plt.show()

1. logistic function

    1. code

            def logistic(x):
                return 1.0 / (1 + math.exp(-x))

            # input gets larger and positive it gets closer and closer to 1
            # input gets larger and negative it gets closer and closer to 0

            def logistic_prime(x):
                return logistic(x) * (1 - logistic(x))

            # use this fit a model
            yi = f(xiβ) + εi
            # where f is a logistic function

    1. fit model

            # linear regression => minimizing the sum of squared errors
            # ended up with the β that maximized the likelihood of the data

            # logistic regression => use gradient descent to maximize the likelihood directly
            # need to calculate the likelihood function and its gradient

    1. formular

            p(yi|xi,β) = f(xiβ)^yi(1-f(xiβ))^(1-yi)

            # since if yi is 0 this equals
            1-f(xiβ)

            # if yi is 1 it equals
            f(xiβ)

            # it turns out that it's actually simpler
            # to maximize the `log likelihood`
            logL(β|xi,yi) = yi logf(xiβ) + (1-yi)log(1-f(xiβ))

    1. log is strictly increasing function

            # any beta that maximizes the log likelihood
            # also maximizes the likelihood and vice versa

            def logistic_log_likelihood_i(x_i, y_i, beta):
                if y_i == 1:
                    return math.log(logistic(dot(x_i, beta)))
                else:
                    return math.log(1 - logistic(dot(x_i, beta)))

    1. if assume different data points are independent from one another

            # the overall likelihood is just the product of the individual likelihood
            # means the overall log likelihood is the sum of the individual log likelihood
            def logistic_log_likelihood(x, y, beta):
                return sum(logistic_log_likelihood_i(x_i, y_i, best)
                           for x_i, y_i in zip(x, y))

    1. a little bit of calculus gives us the gradient

            def logistic_log_partial_ij(x_i, y_i, beta, j):
                """here i is the index of the data point,
                j the index of the derivative"""
                return (y_i - logistic(dot(x_i, beta))) * x_i[j]

            def logistic_log_gradient_i(x_i, y_i, beta):
                """the gradient of the log likelihood
                corresponding to the ith data point"""
                return [logistic_log_partial_ij(x_i, y_i, beta, j)
                        for j, _ in enumerate(beta)]

            def logistic_log_gradient(x, y, beta):
                return reduce(vector_add,
                              [logistic_log_gradient_i(x_i, y_i, beta)
                              for x_i, y_i in zip(x, y)])

1. applying the model
