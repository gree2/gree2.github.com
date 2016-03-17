---
layout: post
title: "dsfs 07 hypothesis and inference"
description: ""
category: [python, data science]
tags: [python, data science, hypothesis, inference]
---
{% include JB/setup %}


### e.g. flipping a coin

1. normal distirbution

			def normal_approximation_to_binomial(n, p):
				"""find mu and sigma corresponding to a binomial(n, p)"""
				mu = p * n
				sigma = math.sqrt(p * (1 - p) * n)
				return mu, sigma

	1. whenever a random variable follows a normal distribution

			# we can use `normal_cdf`
			# the normal cdf is the probability
			# the variable is below a threshold
			normal_probability_below = normal_cdf

			# it's above the threshold if it's not below the threshold
			def normal_probability_above(lo, mu=0, sigma=1):
				return 1 - normal_cdf(lo, mu, sigma)

			# it's between if it's less than hi, but not less than lo
			def normal_probability_between(lo, hi, mu=0, sigma=1):
				return normal_cdf(hi, mu, sigma) - normal_cdf(lo, mu, sigma)

			# it's outside if it's not between
			def normal_probability_outside(lo, hi, mu=0, sigma=1):
				return 1 - normal_probability_between(lo, hi, mu, sigma)

	1. we can do the reverse

			def normal_uper_bound(probability, mu=0, sigma=1):
				"""returns the z for which P(Z <= z) = probability"""
				return inverse_normal_cdf(probability, mu, sigma)

			def normal_lower_bound(probability, mu=0, sigma=1):
				"""returns the z for which P(Z >= z) = probability"""
				return inverse_normal_cdf(1 - probability, mu, sigma)

			def normal_two_sided_bounds(probability, mu=0, sigma=1):
			    """returns the symmetric (about the mean) bounds
			    that contain the specified probability"""
			    tail_probability = (1 - probability) / 2

			    # upper bound should have tail_probability above it
			    upper_bound = normal_lower_bound(tail_probability, mu, sigma)

			    # lower bound should have tail_probability below it
			    lower_bound = normal_upper_bound(tail_probability, mu, sigma)

			    return lower_bound, upper_bound

	1. filp coin n = 1000 times

			mu_0, sigma_0 = normal_approximation_to_binomial(1000, 0.5)

			normal_two_sided_bounds(0.95, mu_0, sigma_0)  # (469, 531)

	1. calculate the power of the test

			# 95% bounds based an assumption p is 0.5
			lo, hi = normal_two_sided_bounds(0.95, mu_0, sigma_0)

			# actual mu and sigma based on p = 0.55
			mu_1, sigma_1 = normal_approximation_to_binomial(1000, 0.55)

			# a type 2 error means we fail to reject the null hypothesis
			# which will happen when X is still in our original interval
			type_2_probability = normal_probability_between(lo, hi, mu_1, sigma_1)
			power = 1 - type_2_probability  # 0.887