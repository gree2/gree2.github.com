---
layout: post
title: "dsfs 13 naive bayes"
description: ""
category: [python, data science]
tags: [python, data science, naive bayes]
---
{% include JB/setup %}


### a real dumb spam filter

            # S => the message is spam
            # V => the message contains the word `viagra`

            # the message is spam conditional on containing the word `viagra` is
            # => P(S|V) = [P(V|S)P(S)]/[P(V|S)P(S)+P(V|¬S)P(¬S)]

            # if have large collection of messages are spam
            # if have large collection of messages are not spam
            # => easily estimate P(V|S) and P(V|¬S)

            # assume that any message is equally likely
            # to be spam or not-spam
            # => P(S) = P(¬S) = 0.5

            # then
            # => P(S|V) = P(V|S)/[P(V|S)+P(V|¬S)]

            # if 50% of spam messages have the word `viagra`
            # but only 1% of nonspam messages do
            # then any given viagra-containing email is spam is
            # => 0.5/(0.5+0.01) = 98%

### a more sophisticated spam filter

            # vocabulary of many words w1,...,wn
            # Xi       => a message contains the word wi
            # P(Xi|S)  => a spam message contains the ith word
            # P(Xi|¬S) => nonspam message contains the ith word

            # => P(X1=x1,...,Xn=xn|S)=P(X1=x1|S)x...xP(Xn=xn|S)

            # a spam message contains both `viagra` and `rolex`
            # => P(X1=1,X2=1|S) = P(X1=1|S)P(X2=1|S) = .5x.5 = .25

            # assumed away `viagra` and `lolex` actually never occur together

            # `viagra-only` spam filter
            # => P(S|X=x) = P(X=x|S)/[P(X=x|S)+P(X=x)|¬S]

            # algebra log(ab) = log(a) + log(b)
            #         exp(log(x)) = x
            # => p1 * ... * pn = exp(log(p1)+...+log(pn))

### implementation