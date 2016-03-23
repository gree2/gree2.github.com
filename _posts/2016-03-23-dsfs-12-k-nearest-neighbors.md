---
layout: post
title: "dsfs 12 k nearest neighbors"
description: ""
category: [python, data science, k-nearest]
tags: [python, data science, k-nearest]
---
{% include JB/setup %}


### the model

1. is one of the simplest predictive models

    1. some notion of distance

    1. an assumption that points that are close to one another are similar

1. e.g.

    1. count votes

            def raw_majority_vote(labels):
                votes = Counter(labels)
                winner, _ votes.most_common(1)[0]
                return winner

    1. options

            # 1. pick one of the winner at random
            # 2. weight the votes by distance and pick the weight winner
            # 3. reduce `k` until find a unique winner

            def majority_vote(labels):
                """assume that labels are ordered from nearest to farthest"""
                vote_counts = Counter(labels)
                winner, winner_count = vote_counts.most_common(1)[0]
                num_winners = len([count
                                   for count in vote_counts.values()
                                   if count == winner_count])

                if 1 == num_winners:
                    return winner
                else:
                    # try again without the farthest
                    return majority_vote(labels[:-1])

    1. create a classifier

            def knn_classify(k, label_points, new_point):
                """each labeld points should be a pair(point, label)"""

                # order the labeld points from nearest to farthest
                by_distance = sorted(label_points,
                                     key=lambda (point, _): distance(point, new_point))

                # find the labels for the k closest
                k_nearest_labels = [label for _, label in by_distance[:k]]

                # and let them vote
                return majority_vote(k_nearest_labels)

1. example favorite languages

    1. data

            # each entry is ([longitude, latitude], favorite_language)
            cities = [([-122.3, 47.53], 'python'),
                      ([-96.85, 32.85], 'java'),
                      ([-89.33, 43.13], 'r'),
                      # and so on
                      ]

    1. plotting

            # key is language, value is pair (longitudes, latitudes)
            plots = {'java': ([], []), 'python': ([], []), 'r', ([], [])}

            # each language have a different marker and color
            markers = {'java': 'o', 'python': 's', 'r': '^'}
            colors = {'java': 'r', 'python': 'b', 'r': 'g'}

            for (lng, lat), lang in cities:
                plots[lang][0].append(lng)
                plots[lang][1].append(lat)

            # create a scatter series for each language
            for lang, (x, y) in plots.iteritems():
                plt.scatter(x, y, color=colors[lang], marker=markers[lang], label=lang, zorder=0)

            # pretend have a function do this job
            plot_state_borders(plt)

            plt.legend(loc=0)
            plt.axis([-130, -60, 20, 55])
            plt.title('favorite programming languages')
            plt.show()

    1. predict each city

            # try several different values for k
            for k in [1, 3, 5, 7]:
                num_correct = 0
                for city in cities:
                    loc, actual_lang = city
                    other_cities = [other_city
                                    for other_city in cities
                                    if other_city != city]
                    predicted_lang = knn_classify(k, other_cities, location)
                    if predicted_lang == actual_lang:
                        num_correct += 1
                print k, 'neighbor[s]:', num_correct, 'correct out of', len(cities)

            # 1 neighbor[s]: 40 correct out of 75
            # 3 neighbor[s]: 44 correct out of 75
            # 5 neighbor[s]: 41 correct out of 75
            # 7 neighbor[s]: 35 correct out of 75