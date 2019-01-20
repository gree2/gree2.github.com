---
layout: post
title: "dsfs 04 linear algebra"
description: ""
category: [python, data science]
tags: [python, data science, vector, matrix]
---
{% include JB/setup %}


### vectors

1. 3-d space

            height_weight_age = [70,  # inches
                                 170, # pounds
                                 40   # years
            ]

            grades = [95, # exam1
                      80, # exam2
                      75, # exam3
                      60  # exam4
            ]

1. adding 2 vectors

            [1, 2] + [2, 1]
            # results in
            [1 + 2, 2 + 1]
            # or
            [3, 3]

            def vector_add(v, w):
                """adds corresponding elements"""
                return [v_i + w_i
                        for v_i, w_i in zip(v, w)]

1. substract

            def vector_substract(v, w):
                """substracts corresponding elements"""
                return [v_i - w_i
                        for v_i, w_i in zip(v, w)]

1. sum

    1. use for loop

            def vector_sum(vectors):
                """sums all corresponding elements"""
                result = vectors[0]
                for vector in vectors[1:]:
                    result = vector_add(result, vector)
                return result

    1. use reduce

            def vector_sum(vectors):
                return reduce(vector_add, vectors)

    1. partial

            vector_sum = partial(reduce, vector_add)

1. multiply

            def scalar_multiply(c, v):
                """c is a number v is a vector"""
                return [c * v_i for v_i in v]

1. means of a list

            def vector_mean(vectors):
                """compute the vector whose ith element
                is the mean of the ith elements of the input vectors"""
                n = len(vectors)
                return scalar_multiply(1 / n, vector_sum(vectors))

1. dot product

            def dot(v, w):
                """v_1 * w_1 + ... + v_n * w_n"""
                return sum(v_i * w_i
                           for v_i, w_i in zip(v, w))

1. sum of squares

            def sum_of_squares(v):
                """v_1 * v_1 + ... + v_n * v_n"""
                return dot(v, v)

1. compute its magnitude or length

            import math

            def magnitude(v):
                return math.sqrt(sum_of_squares(v))

1. compute distance between 2 vectors

            def squared_distance(v, w):
                """(v_1 - w_1) ** 2 + ... + (v_n - w_n) ** 2"""
                return sum_of_squares(vector_substract(v, w))

            def distance(v, w):
                return math.sqrt(squared_distance(v, w))

            def distance(v, w):
                return magnitude(vector_substract(v, w))

### matrix

1. e.g.

            A = [[1, 2, 3],
                 [4, 5, 6]]

            B = [[1, 2],
                 [3, 4],
                 [5, 6]]

1. shape

            def shape(A):
                num_rows = len(A)
                num_cols = len(A[0]) if A else 0
                return num_rows, num_cols

1. get row column

            def get_row(A, i):
                return A[i]

            def get_column(A, j):
                return [A_i[j] for A_i in A]

1. make a matirx

            def make_matirx(num_rows, num_cols, entry_fn):
                """returns a num_rows x num_cols matrix
                whose (i, j)th entry is entry_fn(i, j)"""
                return [[entry_fn(i, j)
                         for j in range(num_cols)]
                        for i in range(num_rows)]

1. identity matrix

            def is_diagonal(i, j):
                """ 1's on the diagonal, 0's everywhere else"""
                return 1 if i == j else 0

            identity_matirx = make_matrix(5, 5, is_diagonal)

            # [[1, 0, 0, 0, 0],
            #  [0, 1, 0, 0, 0],
            #  [0, 0, 1, 0, 0],
            #  [0, 0, 0, 1, 0],
            #  [0, 0, 0, 0, 1]]

1. represent binary relationships

            friendships = [(0, 1), (0, 2), (1, 2), (1, 3), (2, 3), (3, 4),
                           (4, 5), (5, 6), (5, 7), (6, 8), (7, 8), (8, 9)]


            #          user 0  1  2  3  4  5  6  7  8  9
            #
            friendships = [[0, 1, 1, 0, 0, 0, 0, 0, 0, 0], # user 0
                           [1, 0, 1, 0, 0, 0, 0, 0, 0, 0], # user 1
                           [1, 1, 0, 1, 0, 0, 0, 0, 0, 0], # user 2
                           [0, 1, 1, 0, 1, 0, 0, 0, 0, 0], # user 3
                           [0, 0, 0, 1, 0, 1, 0, 0, 0, 0], # user 4
                           [0, 0, 0, 0, 1, 0, 1, 1, 0, 0], # user 5
                           [0, 0, 0, 0, 0, 1, 0, 0, 1, 0], # user 6
                           [0, 0, 0, 0, 0, 1, 0, 0, 1, 0], # user 7
                           [0, 0, 0, 0, 0, 0, 1, 1, 0, 1], # user 8
                           [0, 0, 0, 0, 0, 0, 0, 0, 1, 0], # user 9

    1. much quicker check whether 2 nodes are connected

            friendships[0][2] == 1 # True
            friendships[0][8] == 1 # False

### reference

1. [partial]({% post_url 2016/2016-04-17-python-functools-partial %})
