---
layout: post
title: "dsfs 18 neural networks"
description: ""
category: [python, data science]
tags: [python, data science, neural networks]
---
{% include JB/setup %}


### neural networks

1. artificial neural network => neural network for short

1. the simplest neural network => perceptron

    1. code

            def step_function(x):
                return 1 if x >= 0 else 0

            def perceptron_output(weights, bias, x):
                """return 1 if the perceptron `fires`, 0 if not"""
                calculation = dot(weights, x) + bias
                return step_function(calculation)

    1. `and` gate

            weights = [2, 2]
            bias = 3

            # input  1 1
            # output 2 + 2 - 3 = 1  => 1

            # input  1 0 or 0 1
            # output 2 + 0 - 3 = -1 => 0
            # output 0 + 2 - 3 = -1 => 0

            # input  0 0
            # output 0 + 0 - 3 = -3 => 0

    1. `or` gate

            weights = [2, 2]
            bias = -1

            # input  1 1
            # output 2 + 2 - 1 = 3  => 1

            # input  1 0 or 0 1
            # output 2 + 0 - 1 = 1  => 1
            # output 0 + 2 - 1 = 1  => 1

            # input  0 0
            # output 0 + 0 - 1 = -1 => 0

    1. `not` gate

            weights = [-2]
            bias = 1

            # input  1
            # output -2 + 1 = -1 => 0

            # input  0
            # output 0 + 1 = 1   => 1

    1. single perceptron can't solve

            xor gate

    1. other way to build a logic gate

            and_gate = min
            or_gate = max
            xor_gate = lambda x, y: 0 if x == y else 1

1. feed-forward neural networks

