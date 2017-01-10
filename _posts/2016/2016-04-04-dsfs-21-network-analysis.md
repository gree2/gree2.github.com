---
layout: post
title: "dsfs 21 network analysis"
description: ""
category: [python, data science]
tags: [python, data science, network analysis]
---
{% include JB/setup %}


### network analysis

1. betweenness centrality

            users = [
                { "id": 0, "name": "Hero" },
                { "id": 1, "name": "Dunn" },
                { "id": 2, "name": "Sue" },
                { "id": 3, "name": "Chi" },
                { "id": 4, "name": "Thor" },
                { "id": 5, "name": "Clive" },
                { "id": 6, "name": "Hicks" },
                { "id": 7, "name": "Devin" },
                { "id": 8, "name": "Kate" },
                { "id": 9, "name": "Klein" }
            ]

            friendships = [(0, 1), (0, 2), (1, 2), (1, 3), (2, 3), (3, 4),
                           (4, 5), (5, 6), (5, 7), (6, 8), (7, 8), (8, 9)]

            for user in users:
            user["friends"] = []
            for i, j in friendships:
            # this works because users[i] is the user whose id is i
            users[i]["friends"].append(users[j]) # add i as a friend of j
            users[j]["friends"].append(users[i]) # add j as a friend of i

    1. bfs breadth-first search

            from collections import deque
            def shortest_paths_from(from_user):
                # a dictionary from "user_id" to *all* shortest paths to that user
                shortest_paths_to = { from_user["id"] : [[]] }
                # a queue of (previous user, next user) that we need to check.
                # starts out with all pairs (from_user, friend_of_from_user)
                frontier = deque((from_user, friend)
                                 for friend in from_user["friends"])
                # keep going until we empty the queue
                while frontier:
                    prev_user, user = frontier.popleft() # remove the user who's
                    user_id = user["id"] # first in the queue
                    # because of the way we're adding to the queue,
                    # necessarily we already know some shortest paths to prev_user
                    paths_to_prev_user = shortest_paths_to[prev_user["id"]]
                    new_paths_to_user = [path + [user_id] for path in paths_to_prev_user]
                    # it's possible we already know a shortest path
                    old_paths_to_user = shortest_paths_to.get(user_id, [])
                    # what's the shortest path to here that we've seen so far?
                    if old_paths_to_user:
                        min_path_length = len(old_paths_to_user[0])
                    else:
                        min_path_length = float('inf')
                    # only keep paths that aren't too long and are actually new
                    new_paths_to_user = [path
                                         for path in new_paths_to_user
                                         if len(path) <= min_path_length
                                         and path not in old_paths_to_user]
                    shortest_paths_to[user_id] = old_paths_to_user + new_paths_to_user
                    # add never-seen neighbors to the frontier
                    frontier.extend((user, friend)
                                    for friend in user["friends"]
                                    if friend["id"] not in shortest_paths_to)
                return shortest_paths_to

            # store these dicts with each node
            for user in users:
                user["shortest_paths"] = shortest_paths_from(user)

    1. compute betweenness centrality

            for user in users:
                user["betweenness_centrality"] = 0.0

            for source in users:
                source_id = source["id"]
                for target_id, paths in source["shortest_paths"].iteritems():
                    if source_id < target_id:
                        num_paths = len(paths)
                        contrib = 1 / num_paths
                        for path in paths:
                            for id in path:
                                if id not in [source_id, target_id]:
                                    users[id]["betweenness_centrality"] += contrib

    1. closeness centrality

            def farness(user):
                """the sum of the lengths of the shortest paths to each other user"""
                return sum(len(paths[0])
                           for paths in user["shortest_paths"].values())

            for user in usres:
                user["closeness_centrality"] = 1 / farness(user)

1. eigenvector centrality

            # in order to talk about eigenvector centrality
            #    have to talk about eigenvectors
            # in order to talk about eigenvectors
            #    have to talk about matrix multiplication

    1. matrix multiplication

            # matrix product AB
            Ai1B1j + Ai2B2j + ... + AikBkj

            # just dot product of ith row of A
            #    with jth column of B
            def matrix_product_entry(A, B, i, j):
                return dot(get_row(A, i), get_column(B, j))

            def matrix_multiply(A, B):
                n1, k1 = shape(A)
                n2, k2 = shape(B)
                if k1 != n2:
                    raise ArithmeticError('incompatible shapes!')

                return make_matrix(n1, k2, partial(matrix_product_entry, A, B))

            v = [1, 2, 3]
            v_as_matrix = [[1],
                           [2],
                           [3]]

            def vector_as_matrix():
                """returns the vector v (represented as a list) as a n x 1 matrix"""
                return [[v_i] for v_i in v]

            def vector_from_matrix(v_as_matrix):
                """returns the n x 1 matrix as a list of values"""
                return [row[0] for row in v_as_matrix]

            def matrix_operate(A, v):
                v_as_matrix = vector_as_matrix(v)
                product = matrix_multiply(A, v_as_matrix)
                return vector_from_matrix(product)

            def find_eigenvector(A, tolerance=0.00001):
                guess = [random.random() for _ in A]

                while True:
                    result = matrix_operate(A, guess)
                    length = magnitude(result)
                    next_guess = scalar_multiply(1 / length, result)

                    if distance(guess, next_guess) < tolerance:
                        # eigenvector, eigenvalue
                        return next_guess, length
                    guess = next_guess

            # not all matrices of real numbers have eigenvectors and eigenvalues
            rotate = [[0, 1],
                      [-1, 0]]
            # rotate vector 90 degrees clockwise

    1. centrality

            def entry_fn(i, j):
                return 1 if (i, j) in friendships or (j, i) in friendships else 0

            n = len(users)
            adjacency_matrix = make_matrix(n, n, entry_fn)
            eigenvector_centralities, _ find_eigenvector(adjacency_matrix)

1. directed graphs and pagerank

            endorsements = [(0, 1), (1, 0), (0, 2), (2, 0), (1, 2),
                            (2, 1), (1, 3), (2, 3), (3, 4), (5, 4),
                            (5, 6), (7, 5), (6, 8), (8, 7), (8, 9)]

            for user in users:
                user['endorses'] = []
                user['endorsed_by'] = []

            for source_id, target_id in endorsements:
                user[source_id]['endorses'].append(user[target_id])
                user[target_id]['endorsed_by'].append(user[source_id])

            endorsements_by_id = [(user['id'], len(user['endorsed_by']))
                                  for user in users]

            sorted(endorsements_by_id,
                   key=lambda (user_id, num_endorsements): num_endorsements,
                   reverse=True)

    1. page rank

            def page_rank(users, damping=0.85, num_iters=100):
                # initially distirbute pagerank evenly
                num_users = len(users)
                pr = { user['id'] : 1 / num_users for user in users }

                # this is the small fraction of pagerank
                # that each node gets each iteration
                base_pr = (1 - damping) / numbers

                for _ in range(num_iters):
                    next_pr = { user['id']: base_pr for user in users }
                    for user in users:
                        # distribute pagerank to outgoing links
                        links_pr = pr[user['id']] * damping
                        for endorsee in user['endorses']:
                            next_pr[endorsee['id']] += links_pr / len(user['endorses'])
                    pr = next_pr

                return pr

### reference

1. [partial]({% post_url 2016-04-17-python-functools-partial %})