---
layout: post
title: "dsfs 22 recommender systems"
description: ""
category: [python, data science]
tags: [python, data science, recommender system]
---
{% include JB/setup %}


### recommender systems

1. e.g.

            netflix => movie    => to watch
            amazon  => products => to buy
            twitter => users    => to follow

1. data set `users_interests`

            users_interests = [
                ["Hadoop", "Big Data", "HBase", "Java", "Spark", "Storm", "Cassandra"],
                ["NoSQL", "MongoDB", "Cassandra", "HBase", "Postgres"],
                ["Python", "scikit-learn", "scipy", "numpy", "statsmodels", "pandas"],
                ["R", "Python", "statistics", "regression", "probability"],
                ["machine learning", "regression", "decision trees", "libsvm"],
                ["Python", "R", "Java", "C++", "Haskell", "programming languages"],
                ["statistics", "probability", "mathematics", "theory"],
                ["machine learning", "scikit-learn", "Mahout", "neural networks"],
                ["neural networks", "deep learning", "Big Data", "artificial intelligence"],
                ["Hadoop", "Java", "MapReduce", "Big Data"],
                ["statistics", "R", "statsmodels"],
                ["C++", "deep learning", "artificial intelligence", "probability"],
                ["pandas", "R", "Python"],
                ["databases", "HBase", "Postgres", "MySQL", "MongoDB"],
                ["libsvm", "regression", "support vector machines"]
            ]

1. recommending what's popular

    1. easy approach is to simply recommend what's popular

            popular_interests = Counter(interest
                                        for user_interests in users_interests
                                        for interest in user_interests).most_common()

            [('Python', 4),
             ('R', 4),
             ('Java', 3),
             ('regression', 3),
             ('statistics', 3),
             ('probability', 3),
             # ...
            ]

             # just suggest to a user
             # the most popular interests
             # that he's not already interested in

             def most_popular_new_interests(user_interests, max_results=5):
                suggesstion = [(interest, frequency)
                               for interest, frequency in popular_interests
                               if interest not in user_interests]
                return suggesstion[:max_results]

            # if user 1 with interests
            ["NoSQL", "MongoDB", "Cassandra", "HBase", "Postgres"]

            # recommend
            most_popular_new_interests(users_interests[1], 5)
            # [('Python', 4), ('R', 4), ('Java', 3), ('regression', 3), ('statistics', 3)]

1. user-based collaborative filtering

    1. look for users who are somewhow `similar` to him

            # need a way to measure how similar two users are
            # cosine similarity
            def consine_similarity(v, w):
                return dot(v, w) / math.sqrt(dot(v, v) * dot(w, w))

            # it measures the `angle` between `v` and `w`
            # if `v` and `w` point in the same direction
            #   then the numerator and denominator are equal
            #   and their consine similarity equals 1
            #########################################
                            very similar

            # if `v` and `w` point in opposite direction
            #   their consine similarity equals -1

            # if `v` or `w` is 0
            #   their consine similarity equals 0
            #####################################
                            not very similar

    1. steps

            # 1. unique_interests
            unique_interests = sorted(list({interest
                                            for user_interests in users_interests
                                            for interest in user_interests}))

            # list
            ['Big Data',
             'C++',
             'Cassandra',
             'HBase',
             'Hadoop',
             'Haskell',
             # ...
            ]

            # 2. produce an `interest` vector of 0s and 1s for each user
            def make_user_interest_vector(user_interests):
                """given a list of interests
                produce a vector whose ith element is 1
                if unique_interests[i] is in the list, 0 otherwise"""
                return [1 if interest in user_interests else 0
                        for interest in unique_interests]

            # 3. create a matrix
            user_interest_matrix = map(make_user_interest_vector, users_interests)
            # user_interest_matrix[i][j] == 1 if i specified interest j
            #                               0 otherwise

            # 4. compute pairwise similarities
            user_similarities = [[consine_similarity(interest_vector_i, interest_vector_j)
                                  for interest_vector_j in user_interest_matrix]
                                 for interest_vector_i in user_interest_matrix]

            # user_similarities[0][0] == 0.57
            #    share interests in `hadoop, java, big data`
            # user_similarities[0][8] == 0.19
            #    share only one interest `big data`

            # 5. user_similarities[i] is the vector of i's similarities to every other user
            # finds the most similar users to a given user
            #    not to include the user herself
            #    nor any users with zero similarity
            #    sort the results from most similar to least similar
            def most_similar_users_to(user_id):
                pairs = [(other_user_id, similarity)
                         for other_user_id, similarity in enumerate(user_similarities[user_id])
                         if user_id != other_user_id and similarity > 0]
                return sorted(pairs,
                              key=lambda (_, similarity): similarity,
                              reverse=True)

            [(9, 0.5669467095138409),
             (1, 0.3380617018914066),
             (8, 0.1889822365046136),
             (13, 0.1690308509457033),
             (5, 0.1543033499620919)]

            # 6. suggest new interests to a user
            def user_based_suggestions(user_id, include_current_interests=False):
                suggesstions = defaultdict(float)
                for other_user_id, similarity in most_similar_users_to(user_id):
                    for interest in users_interests[other_user_id]:
                        suggesstion[interest] += similarity

                # convert them to a sorted list
                suggesstions = sorted(suggesstions.items(),
                                      key=lambda (_, weight): weight,
                                      reverse=True)

                # and (maybe) exclude already-interests
                if include_current_interests:
                    return suggesstions
                else:
                    return [(suggesstion, weight)
                            for suggesstion, weight in suggesstions
                            if suggesstion not in users_interests[user_id]]

            # user_based_suggesstion(0)
            [('MapReduce', 0.5669467095138409),
             ('MongoDB', 0.50709255283711),
             ('Postgres', 0.50709255283711),
             ('NoSQL', 0.3380617018914066),
             ('neural networks', 0.1889822365046136),
             ('deep learning', 0.1889822365046136),
             ('artificial intelligence', 0.1889822365046136),
             #...
            ]

1. item-based collaborative filtering
