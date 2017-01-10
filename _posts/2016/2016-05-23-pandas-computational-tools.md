---
layout: post
title: "pandas computational tools"
description: ""
category: [python]
tags: [pandas, statistical, window, aggregation, expanding window, exponentially weighted window]
---
{% include JB/setup %}


### [computational tools](http://pandas.pydata.org/pandas-docs/stable/computation.html)

1. statistical functions

    1. percent change

    1. covariance

    1. correlation

    1. data ranking

1. window functions

    1. summary

            s = pd.Series(np.random.randn(1000), index=pd.date_range('1/1/2000', periods=1000))
            s = s.cumsum()

            r = s.rolling(window=60)
            # r.agg         r.apply       r.count       r.exclusions  r.max         r.median      r.name        r.skew        r.sum
            # r.aggregate   r.corr        r.cov         r.kurt        r.mean        r.min         r.quantile    r.std         r.var

            r.mean()

    1. rolling windows

    1. centering windows

    1. binary window functions

    1. computing rolling pairwise covariance and correlation

1. aggregation

    1. applying multiple functions at once

    1. applying differenct functions to df columns

1. expanding windows

1. exponentially weighted windows

### reference

1. [rolling window demo](https://github.com/gree2/hobby/blob/master/python/pda/moving.average.py)
