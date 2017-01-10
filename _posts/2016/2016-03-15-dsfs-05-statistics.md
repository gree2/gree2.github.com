---
layout: post
title: "dsfs 05 statistics"
description: ""
category: [python, data science]
tags: [python, data science, statistics]
---
{% include JB/setup %}


### desc a single set of data

1. the data itself

            num_friends = [100, 49, 41, 40, 25,
                           # ... and lots more
                          ]

    1. plt.bar()

            friend_counts = Counter(num_friends)
            xs = range(101)
            ys = [friend_counts[x] for x in xs]

            plt.bar(xs, yx)
            plt.axis([0, 101, 0, 25])
            plt.title('histogram of friend counts')
            plt.xlabel('# of friends')
            plt.ylabel('# of people')
            plt.show()

    1. simplest statistic

            num_points = len(num_friends)
            largest_value = max(num_friends)
            smallest_value = min(num_friends)

            # position
            sorted_value = sorted(num_friends)
            sorted_value[0]  # 1 smallest
            sorted_value[1]  # 1 second smallest
            sorted_value[-2] # 49 second largest

1. central tendencies

    1. mean

            from __future__ import division

            def mean(x):
                return sum(x) / len(x)

            mean(num_friends)

    1. median

            def median(v):
                """finds the 'middle-most' value of v"""
                n = len(v)
                sorted_v = sorted(v)

                midpoint = n // 2

                if 1 == n % 2:
                    return sorted_v[midpoint]
                else:
                    lo = midpoint - 1
                    hi = midpoint
                    return (sorted_v[lo] + sorted_v[hi]) / 2

            median(num_friends)

    1. quantile

            def quantile(x, p):
                """returns the pth-percentile value in x"""
                p_index = int(p * len(x))
                return sorted(x)[p_index]

            quantile(num_friends, 0.01)
            quantile(num_friends, 0.25)
            quantile(num_friends, 0.75)
            quantile(num_friends, 0.90)

    1. mode

            def mode(x):
                """returns a list, might be more than one mode"""
                counts = Counter(x)
                max_count = max(counts.values())
                return [x_i for x_i, count in counts.iteritems()
                        if count == max_count]

            mode(num_friends) # 1 and 6

1. dispersion

    1. data range

            def data_range(x):
                """difference between the largest and the smallest"""
                return max(x) - min(x)

            data_range(num_friends) # 99

    1. variance

            def de_mean(x):
                """translate x by substracting its mean
                so the result has mean 0"""
                x_bar = mean(x)
                return [x_i - x_bar for x_i in x]

            def variance(x):
                """assumes x has at least 2 elements"""
                n = len(x)
                deviations = de_mean(x)
                return sum_of_squares(deviations) / (n - 1)

            variance(num_friends)  # 81.54

    1. standard deviations

            def standard_deviation(x):
                return math.sqrt(variance(x))

            standard_deviation(num_friends)  # 9.03

    1. difference between 75th percentile value and 25th

            def interquartile_range(x):
                return quantile(x, 0.75) - quantile(x, 0.25)

            interquartile_range(num_friends)  # 6

1. correlation

    1. covariance

            def covariance(x, y):
                n = len(x)
                return dot(de_mean(x), de_mean(y)) / (n - 1)

            covariance(num_friends, daily_minutes)  # 22.43

    1. correlation

            def correlation(x, y):
                stdev_x = standard_deviation(x)
                stdev_y = standard_deviation(y)
                if stdev_x > 0 and stdev_y > 0:
                    return covariance(x, y) /stdev_x /stdev_y
                else:
                    return 0  # if no variance, correlation is zero

            correlation(num_friends, daily_minutes)  # 0.25

            # the correlation is unitless and aways lies between
            # -1 (perfect anti-correlation) and
            # 1 ( perfect correlation)

            outlier = num_friends.index(100)  # index of outlier

            num_friends_good = [x
                                for i, x in enumerate(num_friends)
                                if i != outlier]

            daily_minutes_good = [x
                                  for i, x in enumerate(daily_minutes)
                                  if i!= outlier]

            correlation(num_friends_good, daily_minutes_good)  # 0.57

1. simpson's paradox

1. some other correlational caveats

1. correlation and causation