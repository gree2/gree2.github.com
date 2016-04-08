---
layout: post
title: "dsfs 24 mapreduce"
description: ""
category: [python, data science]
tags: [python, data science, mapreduce]
---
{% include JB/setup %}


### mapreduce

1. algorithm

    1. use `mapper` function

            turn each item into zero or more `key-value` pairs

    1. collect together all the pairs with identical keys

    1. use `reducer` function

            on each collection of grouped values
            to produce output values for the corresponding key

1. e.g. word count

    1. old fashion

            def word_count_old(documents):
                """word count not using mapreduce"""
                return Counter(word
                               for document in documents
                               for word in tokenize(document))

    1. mapper

            def wc_mapper(document):
                """for each word in the document, emit (word, 1)"""
                for word in tokenize(document):
                    yield (word, 1)

    1. reducer

            def wc_reducer(word, counts):
                """sum up the counts for a word"""
                yield (word, sum(counts))

    1. collect the result from `wc_mapper` and feed them to `wc_reducer`

            def word_count(documents):
                """count the words in the input documents using mapreduce"""
                
                # place to store grouped values
                collector = defaultdict(list)

                for document in documents:
                    for word, count in wc_mapper(document):
                        collector[word].append(count)

                return [output
                        for word, counts in collector.iteritems()
                        for output in wc_reducer(word, counts)]

    1. test

            # 1. three documents
            ['data science', 'big data', 'science fiction']

            # 2. apply wc_mapper
            {
                'data': [1, 1],
                'science': [1, 1],
                'big': [1],
                'fiction': [1]
            }

            # 3. apply wc_reducer
            [
                ('data', 2),
                ('science', 2),
                ('big', 1),
                ('fiction', 1),
            ]

1. why mapreduce

    1. moving processing to data

    1. billions of documents are scattered across 100 machines

            # 1. each machine run the mapper on its documents, procuding lots of (key, value) pairs
            # 2. distribute those paris to a number of `reducing` machines
            #    making sure that the pairs corresponding to any given key all end up on the same machine
            # 3. each reducing machine group the pairs by key and then run the reducer on each set of values
            # 4. return each (key, output) pair

1. mapreduce more generally

    1. general framework

            def map_reduce(inputs, mapper, reducer):
                """runs mapreduce on the inputs using mapper and reducer"""
                collector = defaultdict(list)

                for input in inputs:
                    for key, value in mapper(input):
                        collector[key].append(value)

                return [output
                        for key, values in collector.iteritems()
                        for output in reducer(key, values)]

    1. e.g.

            word_counts = map_reduce(documents, wc_mapper, wc_reducer)

    1. aggregation

            def reduce_values_using(aggregation_fn, key, values):
                """reduces a key-values pair by applying aggregation_fn to the values"""
                yield (key, aggregation_fn(values))

            def values_reducer(aggregation_fn):
                """turns a function (values -> output) into a reducer
                that maps (key, values) -> (key, output)"""
                return partial(reduce_values_using, aggregation_fn)

    1. e.g.

            sum_reducer = values_reducer(sum)
            max_reducer = values_reducer(max)
            min_reducer = values_reducer(min)
            count_distinct_reducer = values_reducer(lambda values: len(set(values)))

1. e.g. analyzing status updates