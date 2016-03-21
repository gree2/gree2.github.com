---
layout: post
title: "dsfs 10 working with data"
description: ""
category: [python, data science]
tags: [python, data science, data]
---
{% include JB/setup %}


### exploring your data

1. 1d

    1. summary statistics

            # count, min, max, mean, standard deviation
            # histogram, discrete buckets

            def bucketize(point, bucket_size):
                """floor the point to the next lower multiple of bucket_size"""
                return bucket_size * math.floor(point / bucket_size)

            def make_histogram(points, bucket_size):
                """buckets the points and counts how many in each bucket"""
                return Counter(bucketize(point, bucket_size) for point in points)

            def plot_histogram(points, bucket_size, title=''):
                histogram = make_histogram(points, bucket_size)
                plt.bar(histogram.keys(), histogram.values(), width=bucket_size)
                plt.title(title)
                plt.show()

    1. demo

            random.seed(0)

            # uniform between -100 and 100
            uniform = [200 * random.random() -100 for _ in range(10000)]

            # normal distribution with mean 0, standard deviation 57
            normal = [57 * inverse_normal_cdf(random.random())
                      for _ in range(10000)]

            # both have means close to 0
            # and standard deviation close to 58
            # however they have very differen distribution

            plot_histogram(uniform, 10, 'uniform histogram')
            plot_histogram(normal, 10, 'normal histogram')
