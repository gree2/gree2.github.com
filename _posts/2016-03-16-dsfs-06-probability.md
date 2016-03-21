---
layout: post
title: "dsfs 06 probability"
description: ""
category: [python, data science]
tags: [python, data science, probability]
---
{% include JB/setup %}


### probability

1. dependence and independence

    1. e and f are independent

            p(e,f) = p(e)p(f)

1. conditional probability

    1. e and f are not independent

            p(e|f) = p(e,f)/p(f)

            # rewrite as this
            p(e,f) = p(e|f)p(f)

            # when e and f are independent
            p(e|f) = p(e)

    1. `b` both children are girls `g` the older child is a girl

            p(b|g) = p(b,g)/p(g) = p(b)/p(g) = 1/2

    1. `b` both children are gilrs `l` at the least one of them is a girl

            p(b|l) = p(b,l)/p(l) = p(b)/p(l) = 1/3

    1. check

            def random_kid():
                return random.choice(['boy', 'girl'])

            both_girls = 0
            older_girl = 0
            either_girl = 0
            random.seed(0)
            for _ in range(10000):
                younger = random_kid()
                older = random_kid()
                if older == 'girl':
                    older_girl += 1
                if older == 'girl' and younger == 'girl':
                    both_girls += 1
                if older == 'girl' or younger == 'girl':
                    either_girl += 1

            print 'p(both | older)', both_girls / older_girl    # 0.514 ~ 1/2
            print 'p(both | either)', both_girls / either_girl  # 0.342 ~ 1/3

1. bayes's theorem

            p(e|f) = p(e,f)/p(f) = p(f|e)p(e)/p(f)

            # given
            p(f) = p(f,e) + p(f,!e)

            # so
            p(e|f) = p(f|e)p(e)/[p(f|e)p(e)+p(f|!e)p(!e)]

### continuous distribution

1. probability density function `pdf`

    1. density function for the uniform distribution

            def uniform_pdf(f):
                return 1 if x >= 0 and x < 1 else 0

1. cumulative distribution function `cdf`

            def uniform_cdf(x):
                """returns the probability that a uniform random variable is <= x"""

                # uniform random is never less than 0
                if x < 0 return 0
                # e.g. P(X <= 0.4) = 0.4
                elif x < 1 return x
                #uniform random is aways less than 1
                else: return 1

### normal distribution
