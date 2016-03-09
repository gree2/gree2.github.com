---
layout: post
title: "apache spark examples"
description: ""
category: [bigdata]
tags: [spark, examples]
---
{% include JB/setup %}


### test search

    file = sc.textFile("hdfs://...")
    errors = file.filter(lambda line: "ERROR" in line)
    errors.count()

    errors.filter(lambda line: "MySQL" in line).count()

    errors.filter(lambda line: "MySQL" in line).collect()

### in memory text search

    file = sc.textFile("hdfs://...")
    errors = file.filter(lambda line: "ERROR" in line)
    errors.cache()
    errors.count()

    errors.filter(lambda line: "MySQL" in line).count()

    errors.filter(lambda line: "MySQL" in line).collect()

### word count

    file = sc.textFile("hdfs://...")
    counts = file.flatMap(lambda line: line.split(" ")) \
    .map(lambda word: (word, 1))                        \
    .reduceByKey(lambda a, b: a + b)

    counts.saveAsTextFile("hdfs://...")

### estimating pi

    def sample(p):
        x, y = random(), random()
        return 1 if x * x + y * y < 1 else 0

    count = sc.parallelize(xrange(0, NUM_SAMPLES)) \
    .map(sample)                                   \
    .reduce(lambda a, b: a + b)

    print "pi is roughly %f" % ( 4.0 * count / NUM_SAMPLES)

### logistic regression

    points = sc.textFile(...).map(parsePoint).cache()
    w = np.random.ranf(size = D)
    for i in range(ITERATIONS):
        gradient = points.map(lambda p: ( 1 / ( 1 + exp( -p.y * (w.dot(p.x)))) -1) * p.y * p.x)
        .reduce(lambda a, b: a + b)
        w -= gradient

    print "final separating plane: %s" % w