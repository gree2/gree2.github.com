---
layout: post
title: "string comparison in python"
description: ""
category: [python]
tags: [Levenshtein, Hamming, Damerau–Levenshtein, Jaro–Winkler, distance, difflib, fuzzywuzzy]
---
{% include JB/setup %}


### question [Find the similarity percent between two strings](http://stackoverflow.com/questions/17388213/find-the-similarity-percent-between-two-strings)

1. question

        similar("Apple","Appel") => 80%
        similar("Apple","Mango") =>  0%

1. answer

        from difflib import SequenceMatcher

        def similar(a, b):
            return SequenceMatcher(None, a, b).ratio()

        >>> similar("Apple","Appel")
        0.8
        >>> similar("Apple","Mango")
        0.0

1. reference [Fuzzy string comparison in Python, confused with which library to use [closed]](http://stackoverflow.com/questions/6690739/fuzzy-string-comparison-in-python-confused-with-which-library-to-use)

    1. question

            import Levenshtein
            Levenshtein.ratio('hello world', 'hello')

            Result: 0.625

            import difflib
            difflib.SequenceMatcher(None, 'hello world', 'hello').ratio()

            Result: 0.625

    1. answer

            difflib.SequenceMatcher => Ratcliff/Obershelp algorithm
            Levenshtein             => Levenshtein algorithm

### [FuzzyWuzzy: Fuzzy String Matching in Python](http://chairnerd.seatgeek.com/fuzzywuzzy-fuzzy-string-matching-in-python/)

1. String Similarity

1. Partial String Similarity

### references

1. distance

    1. [Hamming distance](http://en.wikipedia.org/wiki/Hamming_distance)

    1. [Levenshtein distance](http://en.wikipedia.org/wiki/Levenshtein_distance)

    1. [Damerau–Levenshtein distance](http://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance)

    1. [Jaro–Winkler distance](http://en.wikipedia.org/wiki/Jaro%E2%80%93Winkler_distance)

1. source code

    1. [Levenshtein.c](http://code.google.com/p/pylevenshtein/source/browse/trunk/Levenshtein.c)
