---
layout: post
title: "dsfs 11 machine learning"
description: ""
category: [python, data science]
tags: [python, data science, machine learning]
---
{% include JB/setup %}


### machine learning

1. modeling

            mathematical (or probabilistic) relationship
            that exists between different variables

1. what is machine learning

            use existing data to `develop models`
            that can use to `predict` various outcomes for new data

            predicting whether an email message is spam or not
            predicting whether a credit card transaction is fraudulent
            predicting which advertisement a shopper is most likely to click on
            predicting which football team is going to win the super bowl

1. overfitting and underfitting

    1. simplest way is split data set

            def split_data(data, prob):
                """split data into fractions [prob, 1 - prob]"""
                result = [], []
                for row in data:
                    result[0 if random.random() < prob else 1].append(row)
                return result

    1. training data or test data

            def train_test_split(x, y, test_pct):
                data = zip(x, y)
                train, test = split_data(data, 1 - test_pct)
                x_train, y_train = zip(*train)
                x_test, y_test = zip(*test)
                return x_train, x_test, y_train, y_test

            # do something like
            model = SomeKindOfModel()
            x_train, x_test, y_train, y_test = train_test_split(xs, ys, 0.33)
            performace = model.test(x_test, y_test)

### correctness

1. every data lies in one of four categories

    1. true positive

            this message is spam, and correctly predicted spam

    1. false positive

            type 1 error: not spam but predicted spam

    1. false negative

            type 2 error: is spam but predicted not spam

    1. true negative

            is not spam correctly predicted not spam

1. confusion matrix

            +------------------+---------------+----------------+
            |                  | is spam       | is not spam    |
            +------------------+---------------+----------------+
            | predict spam     | true positive | false positive |
            +------------------+---------------+----------------+
            | predict not spam | true positive | false positive |
            +------------------+---------------+----------------+

1. `accuracy` is defined as the fraction of correct prediction

            def accuracy(tp, fp, fn, tn):
                correct = tp + tn
                total = tp + fp + fn + tn
                return correct / total

            print accuracy(70, 4940, 13930, 981070)  # 0.98114

    1. it's common to look at the combination of `precision` and `recall`

            def precision(tp, fp, fn, tn):
                return tp / (tp + fp)

            print precision(70, 4940, 13930, 981070)  # 0.014

            def recall(tp, fp, fn, tn):
                return tp / (tp + fn)

            print recall(70, 4940, 13930, 981070)  # 0.005

            0.014 and 0.005 are both terrible numbers
            reflecting that is a terrible model

    1. precision and recall are combined into the `f1 score`

            def f1_score(tp, fp, fn, tn):
                p = precision(tp, fp, fn, tn)
                r = recall(tp, tp, fn, tn)
                return 2 * p * r / (p + r)

            # this is the `harmonic mean` of precision and recall
            # and necessarily lies between them

### the bias-variance trade-off

1. another way of thinking about `overfitting` is as a trade-off between bias and variance

    1. high bias and low variance typically correspond to underfitting

    1. low bias and high variance typically correspond to overfitting

    1. if your model has high bias

            which means it performs poorly even on your training data
            then one thing to try is adding more features

    1. if your model has high variance

            you can similarly remove features
            but another solution is to obtain more data

### feature extraction and selection

1. features are whatever inputs we provide to our model

    1. when data doesn't have enough features, model is likely to underfit

    1. when data has too many features, it's easy to overfit

1. e.g.

            if you want to predict someone's salay
            based on her `years of experience`
            then `years of experience` if the only feature you have

1. extract features from our data that fall into one of these three categories

    1. naive bayes classifier

            suited to yes-or-no features

    1. regression models

            require numeric features

    1. decision trees

            deal with numeric or categorical data
