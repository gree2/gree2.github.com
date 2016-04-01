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
                "_NP" : ["_N",
                "_A _NP _P _A _N"],
                "_VP" : ["_V",
                "_V _NP"],
                "_N" : ["data science", "Python", "regression"],
                "_A" : ["big", "linear", "logistic"],
                "_P" : ["about", "near"],
                "_V" : ["learns", "trains", "tests", "is"]
            }

            # _S  sentence
            # _NP noun phrase
            # _VP verb phrase