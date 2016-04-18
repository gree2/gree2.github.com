---
layout: post
title: "dsds 20 natural language processing"
description: ""
category: [python, data science]
tags: [python, data science, nlp, natural language processing]
---
{% include JB/setup %}


### nlp

1. word clouds

    1. buzzword cloud

            data = [ ("big data", 100, 15), ("Hadoop", 95, 25), ("Python", 75, 50),
            ("R", 50, 40), ("machine learning", 80, 20), ("statistics", 20, 60),
            ("data science", 60, 70), ("analytics", 90, 3),
            ("team player", 85, 85), ("dynamic", 2, 90), ("synergies", 70, 0),
            ("actionable insights", 40, 30), ("think out of the box", 45, 10),
            ("self-starter", 30, 50), ("customer focus", 65, 15),
            ("thought leadership", 35, 35)]

    1. code

            def text_size(total):
                """equals 8 if total is 0, 28 if total is 200"""
                return 8 + total / 200 * 20

            for word, job_popularity, resume_popularity in data:
                plt.text(job_popularity, resume_popularity, word,
                         ha='center', va='center',
                         size=text_size(job_popularity + resume_popularity))

            plt.xlabel('popularity on job postings')
            plt.ylabel('popularity on resumes')
            plt.axis([0, 100, 0, 100])
            plt.xticks([])
            plt.yticks([])
            plt.show()

1. n-gram models

    1. ch09 retrieve data

            # replace unicode
            def fix_unicode(text):
                return text.replace(u'\u2089', "'")

            # code
            from bs4 import BeautifulSoup
            import requests
            url = 'http://radar.oreilly.com/2010/06/what-is-data-science.html'
            html = requests.get(url).text
            soup = BeautifulSoup(html, 'html5lib')

            content = soup.find('div', 'entry-content')
            regex = r'[\w]+[\.]'

            document = []
            for paragraph in content('p'):
                words = re.findall(regex, fix_unicode(paragraph.text))
                document.extend(word)

    1. zip stops when any of its inputs is done

            # bigrams
            bigrams = zip(document, document[1:])
            transitions = defaultdict(list)
            for prev, current in bigrams:
                transitions[prev].append(current)

            def generate_using_bigrams():
                current = '.'
                result = []
                while True:
                    next_word_candidates = transitions[current]
                    current = random.choice(next_word_candidates)
                    result.append(current)
                    if current == '.': return ' '.join(result)

    1. trigrams triplets of consecutive words

            # n-grams n consecutive words
            trigrams = zip(document, document[1:], document[2:])
            trigrams_transitions = defaultdict(list)

            starts = []
            for prev, current, next in trigrams:
                if prev == '.':
                    starts.append(current)

                trigram_transitions[(prev, current)].append(next)

            def generate_using_trigrams():
                current = random.choice(starts)
                prev = '.'
                result = [current]
                while True:
                    next_word_candidates = trigram_transitions[(prev, current)]
                    next_word = random.choice(next_word_candidates)

                    prev, current = current, next_word
                    result.append(current)

                    if current == '.':
                        return ' '.join(result)

1. grammars

    1. define

            grammar = {
                "_S" : ["_NP _VP"],
                "_NP" : ["_N", "_A _NP _P _A _N"],
                "_VP" : ["_V", "_V _NP"],
                "_N" : ["data science", "Python", "regression"],
                "_A" : ["big", "linear", "logistic"],
                "_P" : ["about", "near"],
                "_V" : ["learns", "trains", "tests", "is"]
            }

            # _S  sentence
            # _NP noun phrase
            # _VP verb phrase

    1. how to generate sentences from this grammar


            ['_S']
            ['_NP','_VP']
            ['_N','_VP']
            ['Python','_VP']
            ['Python','_V','_NP']
            ['Python','trains','_NP']
            ['Python','trains','_A','_NP','_P','_A','_N']
            ['Python','trains','logistic','_NP','_P','_A','_N']
            ['Python','trains','logistic','_N','_P','_A','_N']
            ['Python','trains','logistic','data science','_P','_A','_N']
            ['Python','trains','logistic','data science','about','_A', '_N']
            ['Python','trains','logistic','data science','about','logistic','_N']
            ['Python','trains','logistic','data science','about','logistic','Python']

    1. code

            def is_terminal(token):
                return token[0] != '_'

            def expand(grammar, tokens):
                for i, token in enumerate(tokens):
                    if is_terminal(token):
                        continue
                    replacement = random.choice(grammar[token])
                    if is_terminal(replacement):
                        token[i] = replacement
                    else:
                        tokens = tokens[:i] + replacement.split() + tokens[(i+1):]
                    return expand(grammar, tokens)
                return tokens

            # generating sentences
            def generate_sentence(grammar):
                return expand(grammar, ['_S'])

1. an aside gibbs sampling

    1. generating sampling from some distributions is easy

            random.random()

            # and normal random variables with
            inverse_normal_cdf(random.random())

            # but some distributions are harder to sample from

    1. gibbs sampling is a technique

            # for generating samples
            # from multidimensional distributions
            # when only know some of the conditional distributions

    1. e.g.

            # rolling two dice
            # x => first dice
            # y => sum of the dice
            # generate lots of (x, y)

            def roll_a_dice():
                return random.choice([1, 2, 3, 4, 5, 6])

            def direct_sample():
                d1 = roll_a_dice()
                d2 = roll_a_dice()
                return d1, d1 + d2

            # know the conditional distributions
            def random_y_given_x(x):
                """equally likely to be x + 1, x + 2, ..., x + 6"""
                return x + roll_a_dice()

            # random_x_given_y(y):
                if y <= 7:
                    # if the total is 7 or less
                    # the first dice is equally likely to be
                    # 1, 2, ..., (total - 1)
                    return random.randrange(1, y)
                else:
                    # if the total is 7 or more
                    # the first dice is equally likely to be
                    # (total - 6), (total - 5), ..., 6
                    return random.randrange(y - 6, 7)

            # the way gibbs sampling works
            # start with any (valid) value for x, y
            # repeatedly alternate replacing x
            # with a random value picked conditional on y
            # and y vice versa
            # after a number of iterations
            # the result values of x, y will represent a sample from the unconditional joint distribution
            def gibbs_sample(num_iters=100):
                # doesn't really matter
                x, y = 1, 2
                for _ in range(num_iters):
                    x = random_x_given_y(y)
                    y = random_y_given_x(x)
                return x, y

            # direct sample
            def compare_distributions(num_samples=1000):
                counts = defaultdict(lambda: [0, 0])
                for _ in range(num_samples):
                    counts[gibbs_sample()][0] +=1
                    counts[direct_sample()][1] +=1
                return counts

1. topic modeling

    1. latent dirichlet analysis (lda)

            # lda has some similarities to the naive bayes classifier
            # it assumes a probabilistic model for documents

    1. code

            def sample_from(weights):
                """returns i with probability weights[i] / sum(weights)"""
                total = sum(weights)
                rnd = total * random.random()
                for i, w in enumerate(weights):
                    rnd -= w
                    if rnd <= 0: return i

            documents = [
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

            # a list of counters, one for each document
            document_topic_counts = [Counter() for _ in documents]
            # a list of counters, one for each topic
            topic_word_counts = [Counter() for _ in range(K)]
            # a list of numbers, one for each topic
            topic_counts = [0 for _ in range(K)]
            # a list of numbers, one for each document
            document_lengths = map(len, documents)

            # the number of distinct words
            distinct_words = set(word for document in documents for word in document)
            W = len(distinct_words)
            D = len(documents)

            def p_topic_given_document(topic, d, alpha=0.1):
                """the fraction of words in document d
                that are assigned to topic (plus some smoothing)"""
                return ((document_topic_counts[d][topic] + alpha) /
                        (document_lengths[d] + K * alpha))

            def p_word_given_topic(word, topic, beta=0.1):
                """the fraction of words assigned to topic
                that equal word (plus some smoothing)"""
                return ((topic_word_counts[topic][word] + beta) /
                        (topic_counts[topic] + W * beta))

            def topic_weight(d, word, k):
                """given a document and a word in that document,
                return the weight for the kth topic"""
                return p_word_given_topic(word, K) * p_topic_given_document(k, d)

            def choose_new_topic(d, word):
                return sample_from([topic_weight(d, word, k)
                                    for k in range(K)])

            # assigning every word to a random topic
            # populating our counters
            random.seed(0)
            document_topics = [[random.randrange(K) for word in document]
                               for document in documents]

            for d in range(D):
                for word, topic in zip(documents[d], document_topics[d]):
                    document_topic_count[d][topic] += 1
                    topic_word_counts[topic][word] += 1
                    topic_counts[topic] += 1

            for iter in range(1000):
                for d in range(D):
                    for i, (word, to)