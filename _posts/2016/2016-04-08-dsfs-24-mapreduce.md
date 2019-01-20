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

    1. data

            {"id": 1,
            "username" : "joelgrus",
            "text" : "Is anyone interested in a data science book?",
            "created_at" : datetime.datetime(2013, 12, 21, 11, 47, 0),
            "liked_by" : ["data_guy", "data_gal", "mike"] }

    1. figure out which day of the week

            # people talk the most about data science
            # group by the day of week => key
            def data_science_day_mapper(status_update):
                """yield (day_of_week, 1) if status_update
                contains 'data science'"""
                if "data science" in status_update['text'].lower():
                    day_of_week = status_update['created_at'].weekday()
                    yield (day_of_week, 1)

            data_science_days = map_reduce(status_updates,
                                           data_science_day_mapper,
                                           sum_reducer)

    1. find out each user the most common word

            # 1. put the username in the key;
            #    put the words and counts in the value
            # 2. put the word in the key;
            #    put the usernames and counts in the values
            # 3. put the username and word in the key;
            #    put the coutns in the values

            # the first option is right choice
            def words_per_user_mapper(status_update):
                user = status_update['username']
                for word in tokenize(status_update['text']):
                    yield (user, (word, 1))

            def most_popular_word_reducer(user, words_and_counts):
                """given a sequence of (word, count) pairs,
                return the word with the highest total count"""
                word_count = Counter()
                for word, count in words_and_counts:
                    word_count[word] += count

                word, count = word_count.most_common(1)[0]
                yield (user, (word, count))

            user_words = map_reduce(status_update,
                                    word_per_user_mapper,
                                    most_popular_word_reducer)

    1. find out the number of distinct status-likers for each user

            def liker_mapper(status_update):
                user = status_update['username']
                for liker in status_update['liked_by']:
                    yield (user, liker)

            distinct_likers_per_user = map_reduce(status_update,
                                                  liker_mapper,
                                                  count_distinct_reducer)

1. e.g. matrix multiplication

    1. m x n matrix A and n x k matrix B

            # A x B => m x k matrix C
            # element of C in row i and column j is given by
            Cij = Ai1B1j + Ai2B2j + ... + AinBnj

    1. `natural` way to represent m x n matrix => a list of lists

            # large matrices are sometimes `sparse`
            # list of lists can be a very wasteful representation
            # a more compact representation => list of tuples (name, i, j, value)

    1. code

            def matrix_multiply_mapper(m, element):
                """m is the common dimension (column of A, rows of B)
                element is a tuple (matrix_name, i, j, value)"""

                if name == "A":
                    # A_ij is the jth entry
                    # in the sum for each C_ik, k=1..m
                    for k in range(m):
                        # group with other entries for C_ik
                        yield ((i, k), (j, value))

                else:
                    # B_ij is the ith entry
                    # in the sum for each C_kj
                    for k in range(m):
                        # group with other entries for C_kj
                        yield ((k, j), (i, value))

            def matrix_multiply_reducer(m, key, indexed_values):
                results_by_index = defaultdict(list)
                for index, value in indexed_values:
                    results_by_index[index].append(value)

                sum_product = sum(results[0] * results[1]
                                  for results in results_by_index.values()
                                  if len(results) == 2)

                if sum_product != 0.0
                    yield (key, sum_product)

    1. e.g.

            # two matrices
            A = [[3, 2, 0],
                 [0, 0, 0]]
            B = [[4, -1, 0],
                 [10, 0, 0],
                 [0, 0, 0]]

            # rewrite
            entries = [("A", 0, 0, 3), ("A", 0, 1, 2),
            ("B", 0, 0, 4), ("B", 0, 1, -1), ("B", 1, 0, 10)]
            mapper = partial(matrix_multiply_mapper, 3)
            reducer = partial(matrix_multiply_reducer, 3)
            map_reduce(entries, mapper, reducer) # [((0, 1), -3), ((0, 0), 32)]

1. an aside combiners

            500 instances of ('data', 1)
            => 1 instance of ('data', 500)

### reference

1. [partial]({% post_url 2016/2016-04-17-python-functools-partial %})
