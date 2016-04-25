---
layout: post
title: "dsfs 19 clustering"
description: ""
category: [python, data science]
tags: [python, data science, clustering]
---
{% include JB/setup %}


### clustering

1. the model

    1. one of the simplest clustering methods is k-means

            # the number of clusters k is chosen in advance
            # 1. start with a set of k-means, which are points in d-dimensional space
            # 2. assign each point to the mean to which it is closest
            # 3. if no point's assignment has changed, stop and keep the clusters
            # 4. if some point's assignment has changed, recompute the means and return to step 2

    1. code

            # use `vector_mean` in ch04
            class KMeans:
                """performs k-means clustering"""

                def __init__(self, k):
                    self.k = k
                    self.mean = None

                def classify(self, input):
                    """return the index of the cluster closest to the input"""
                    return min(range(self.k),
                               key=lambda i: squared_distance(input, self.means[i]))

                def train(self, inputs):
                    """choose k random points as the intial means"""
                    self.means = random.sample(inputs, self.k)
                    assignments = None
                    while True:
                        # find new assignments
                        new_assignments = map(self.classify, inputs)

                        # if no assignments have changed, we're done
                        if assignments == new_assignments:
                            return

                        # otherwise keep the new assignments
                        assignments = new_assignment

                        # compute new means based on the new assignments
                        for i in range(self.k):
                            # find all the points assigned to cluster i
                            i_points = [p for p, a in zip(inputs, assignments) if a == i]

                            # make sure i_points is not empty avoid divide by 0
                            if i_points:
                                self.mean[i] = vector_mean(i_points)

    1. e.g. meetups

            # get the same result
            random.seed(0)
            clusterer = KMeans(3)
            clusterer.train(inputs)
            print clusterer.means

            # two meetups
            ...
            clusterer = KMeans(2)
            ...

1. choosing k

            def squared_clustering_errors(inputs, k):
                """find the total squared error from k-means clustering the inputs"""
                clusterer = KMeans(k)
                clusterer.train(inputs)
                means = clusterer.means
                assignments = map(clusterer.classify, inputs)

                return sum(squared_distance(input, means[cluster])
                           for input, cluster in zip(inputs, assignments))

            # plot from 1 up to len(inputs) clusters
            ks = range(1, len(inputs) + 1)
            errors = [squared_clustering_errors(inputs, k) for k in ks]

            plt.plot(ks, errors)
            plt.xticks(ks)
            plt.xlabel('k')
            plt.ylabel('total squared error')
            plt.title('total error vs. # of clusters')
            plt.show()

    1. e.g. clustering colors

            # create a five-color version of an image
            # 1. choose five color
            # 2. assigning one of those colors to each pixel


            # k-means clustering
            # partition the pixels into five clusters in rgb space
            # recolor the pixels in each cluster to the mean color

            # 1. read image into python
            file_png = r'C:\images\image.png'
            import matplotlib.image as mping
            # behind the scenes `img` is a numpy array
            img = mping.imread(file_png)

            # 2. flatten list of all the pixels
            # top_row = img[0]
            # top_left_pixel = top_row[0]
            # red, green, blue = top_left_pixel
            pixels = [pixel for row in img for pixel in row]

            # 3. feed them to our cluster
            clusterer = KMean(5)
            clusterer.train(pixels)

            # 4. construct a new image with the same format
            def recolor(pixel):
                cluster = clusterer.classify(pixel)
                return clusterer.means(cluster)

            new_img = [[recolor(pixel) for pixel in row]
                       for row in img]

            # 5. display it
            plt.imshow(new_img)
            plt.axis('off')
            plt.show()

1. bottom-up hierarchical clustering

    1. grow clusters from the bottom up

            # steps
            # 1. make each input its own cluster of one
            # 2. as long as there are multiple clusters remaining
            #    find the two closest clusters and merge them

    1. representation of clusters

            # to make a 1-tuple need the trailing comma
            # otherwise python treats the parentheses as parentheses
            leaf1 = ([10, 20],)
            leaf2 = ([30, -15],)

    1. use these to grow merged clusters

            # which will represent as 2-tuple
            merged = (1, [leaf1, leaf2])

            def is_leaf(cluster):
                """a cluster is a leaf if it has length 1"""
                return 1 == len(cluster)

            def get_children(cluster):
                """returns the two children of this cluster if it's a merged cluster;
                raise an exception if this is a leaf cluster"""
                if is_leaf(cluster):
                    raise TypeError("a leaf cluster has no children")
                else:
                    return cluster[1]

            def get_values(cluster):
                """returns the value in this cluster (if it's a leaf cluster)
                or all the values in the leaf clusters below it (if it's not)"""
                if is_leaf(cluster):
                    return cluster
                else:
                    return [value
                            for child in get_children(cluster)
                            for value in get_values(child)]

    1. merge the closest clusters

            # need some notion of the distance between clusters
            def cluster_distance(cluster1, cluster2, distance_agg=min):
                """compute all the pairwise distances between cluster1 and cluster2
                and apply _distance_agg_ to the resulting list"""
                return distance_agg([distance(input1, input2)
                                     for input1 in get_values(cluster1)
                                     for input2 in get_values(cluster2)])

            def get_merge_order(cluster):
                # since leaf clusters were never merged
                # assign them infinity
                if is_leaf(cluster):
                    return float('inf')
                else:
                    # merge_order is first element of 2-tuple
                    return cluster[0]

    1. clustering algorithm

            def bottom_up_cluster(inputs, distance_agg=min):
                # start with every input a leaf cluster / 1-tuple
                clusters = [(input, ) for input in inputs]

                # as long as we have more than one cluster left...
                while len(cluster) > 1:
                    # find the two closest clusters
                    c1, c2 = min([(cluster1, cluster2)
                                   for i, cluster1 in enumerate(clusters)
                                   for cluster2 in clusters[:i]],
                                  key=lambda (x, y): cluster_distance(x, y, distance_agg))

                    # remove them from the list of clusters
                    clusters = [c for c in clusters if c != c1 and c!= c2]

                    # merge them, using merge_order = # of the clusters left
                    merged_cluster = (len(clusters), [c1, c2])

                    # add their merge
                    cluster.append(merged_cluster)

                # when there's only one cluster left, return it
                return clusters[0]

    1. e.g.

            base_cluster = bottom_up_cluster(inputs)

    1. generate clusters

            def generate_clusters(base_cluster, num_clusters):
                # start with a list with just the base cluster
                cluster = [base_cluster]

                # as long as we don't have enough clusters yes...
                while len(clusters) < num_clusters:
                    # choose the last-merged of our clusters
                    next_cluster = min(clusters, key=get_merge_order)
                    # remove it form the list
                    clusters = [c for c in clusters if c!= next_cluster]
                    # add its children to the list (i.e., unerge it)
                    clusters.extend(get_children(next_cluster))

                # onec we have enough clusters...
                return clusters

    1. three clusters

            three_clusters = [get_values(cluster)
                              for cluster in generate_clusters(base_cluster, 3)]

            # plot
            for i, cluster, marker, color in zip([1, 2, 3],
                                                 three_clusters,
                                                 ['D', 'o', '*'],
                                                 ['r', 'g', 'b'])
                xs, ys = zip(*cluster)  # magic unzipping trick
                plt.scatter(xs, yx, color=color, marker=marker)

                # put a number at the mean of the cluster
                x, y = vector_mean(cluster)
                plt.plot(x, y, marker='$' + str(i) + '$', color='black')

            plt.title('user locations --- 3 bottom up clusters, min')
            plt.xlabel('blocks east of city center')
            plt.ylabel('blocks north of city center')
            plt.show()

            # min => chain-like clusters
            # max => tight clusters

    1. efficient

            # relatively simple
            # shockingly inefficient

            # 1. recompute distance between each pair of inputs at every step
            #    => precompute the distances
            #       perform a lookup inside cluster_distance

            # 2. remember the cluster_distance from the previous step

### reference

1. [sklearn.cluster](http://scikit-learn.org/stable/modules/clustering.html)