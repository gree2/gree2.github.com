---
layout: post
title: "dsfs 17 decision trees"
description: ""
category: [python, data science]
tags: [python, data science, decision tree]
---
{% include JB/setup %}


### decision tree

1. what is decision tree

    1. a number of possible decision paths

    1. outcome for each path

1. entropy

    1. define

            H(S)= − p1 log2 p1 − ... − pn log2 pn

            # standard conversion
            # 0 log 0 = 0

    1. roll all this into a function

            def entropy(class_probabilities):
                """given a list of class probabilities
                compute the entropy"""
                return sum(-p * math.log(p, 2)
                           for p in class_probabilities
                           if p)  # ignore zero probabilities

            def class_probabilities(labels):
                total_count = len(labels)
                return [count / total_count
                        for count in Counter(labels).values()]

            def data_entropy(labeled_data):
                labels = [label for _, label in labeled_data]
                probabilities = class_probabilities(labels)
                return entropy(probabilities)

1. the entropy of a partition

    1. define

            H = q1H(S1) + ... + qmH(Sm)

    1. implement

            def partition_entropy(subsets):
                """find the entropy from this partition of data into subsets
                subsets is a list of lists of labeled data"""
                total_count = sum(len(subset) for subset in subsets)
                return sum(data_entropy(subset) * len(subset) / total_count
                           for subset in subsets)

1. creating a decision tree

    1. data

            inputs = [
                ({'level':'Senior', 'lang':'Java', 'tweets':'no', 'phd':'no'}, False),
                ({'level':'Senior', 'lang':'Java', 'tweets':'no', 'phd':'yes'}, False),
                ({'level':'Mid', 'lang':'Python', 'tweets':'no', 'phd':'no'}, True),
                ({'level':'Junior', 'lang':'Python', 'tweets':'no', 'phd':'no'}, True),
                ({'level':'Junior', 'lang':'R', 'tweets':'yes', 'phd':'no'}, True),
                ({'level':'Junior', 'lang':'R', 'tweets':'yes', 'phd':'yes'}, False),
                ({'level':'Mid', 'lang':'R', 'tweets':'yes', 'phd':'yes'}, True),
                ({'level':'Senior', 'lang':'Python', 'tweets':'no', 'phd':'no'}, False),
                ({'level':'Senior', 'lang':'R', 'tweets':'yes', 'phd':'no'}, True),
                ({'level':'Junior', 'lang':'Python', 'tweets':'yes', 'phd':'no'}, True),
                ({'level':'Senior', 'lang':'Python', 'tweets':'yes', 'phd':'yes'}, True),
                ({'level':'Mid', 'lang':'Python', 'tweets':'no', 'phd':'yes'}, True),
                ({'level':'Mid', 'lang':'Java', 'tweets':'yes', 'phd':'no'}, True),
                ({'level':'Junior', 'lang':'Python', 'tweets':'no', 'phd':'yes'}, False)
            ]

    1. tree

            # decision nodes => ask question and direct differently depending on the answer
            # leaf nodes     => give a prediction

            # we will build it using the relatively simple id3 algorithm

            # 1. if the data all have the same label
            #    then create a leaf node that predicts that label and then stop

            # 2. if the list of attributes is empty (i.e. there are no more possible question to ask)
            #    then create a leaf node that predicts the most common label and then stop

            # 3. otherwise try partitioning the data by each of the attributes

            # 4. choose the partition with the lowest partition entropy

            # 5. add a decision node based on the chosen attribute

            # 6. recur on each partitioned subset using the remaining attributes

    1. implement

            def partition_by(inputs, attribute):
                """each input is a pair (attribute_dict, label)
                returns a dict: attribute_value -> inputs"""
                groups = defaultdict(list)
                for input in inputs:
                    key = input[0][attribute]
                    groups[key].append(input)
                return groups

            def partition_entropy_by(inputs, attribute):
                """computes the entropy corresponding to the given partition"""
                partitions = partition_by(inputs, attribute)
                return partition_entropy(partitions.values())

            # find the minimum-entropy partition for whole data set
            for key in ['level', 'lang', 'tweets', 'phd']:
                print key, partition_entropy_by(inputs, key)

            # level  0.693536138896
            # lang   0.860131712855
            # tweets 0.788450457308
            # phd    0.892158928262

            # the lowest entropy comes from splitting on `level`
            # make a subtree for each possible level value

            senior_inputs = [(input, label)
                              for input, label in inputs if input['level'] == 'Senior']

            for key in ['lang', 'tweets', 'phd']:
                print key, partition_entropy_by(senior_inputs, key)

            # lang 0.4
            # tweets 0.0
            # phd 0.950977500433

            # next split should be on `tweets`

1. putting it all together