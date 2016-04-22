---
layout: post
title: "dsfs 13 naive bayes"
description: ""
category: [python, data science]
tags: [python, data science, naive bayes]
---
{% include JB/setup %}


### a real dumb spam filter

            # S => the message is spam
            # V => the message contains the word `viagra`

            # the message is spam conditional on containing the word `viagra` is
            # => P(S|V) = [P(V|S)P(S)]/[P(V|S)P(S)+P(V|¬S)P(¬S)]

            # if have large collection of messages are spam
            # if have large collection of messages are not spam
            # => easily estimate P(V|S) and P(V|¬S)

            # assume that any message is equally likely
            # to be spam or not-spam
            # => P(S) = P(¬S) = 0.5

            # then
            # => P(S|V) = P(V|S)/[P(V|S)+P(V|¬S)]

            # if 50% of spam messages have the word `viagra`
            # but only 1% of nonspam messages do
            # then any given viagra-containing email is spam is
            # => 0.5/(0.5+0.01) = 98%

### a more sophisticated spam filter

            # vocabulary of many words w1,...,wn
            # Xi       => a message contains the word wi
            # P(Xi|S)  => a spam message contains the ith word
            # P(Xi|¬S) => nonspam message contains the ith word

            # => P(X1=x1,...,Xn=xn|S)=P(X1=x1|S)x...xP(Xn=xn|S)

            # a spam message contains both `viagra` and `rolex`
            # => P(X1=1,X2=1|S) = P(X1=1|S)P(X2=1|S) = .5x.5 = .25

            # assumed away `viagra` and `lolex` actually never occur together

            # `viagra-only` spam filter
            # => P(S|X=x) = P(X=x|S)/[P(X=x|S)+P(X=x)|¬S]

            # algebra log(ab) = log(a) + log(b)
            #         exp(log(x)) = x
            # => p1 * ... * pn = exp(log(p1)+...+log(pn))

### implementation

1. build classifier

    1. tokenize messages

            def tokenize(message):
                message = message.lower()
                all_words = re.findall("[a-z0-9']+", message)
                return set(all_words)

    1. count the words in a labeled training set of messages

            def count_words(training_set):
                """training set consists of pairs (messages, is_spam)"""
                counts = defaultdict(lambda: [0, 0])
                for message, is_spam in training_set:
                    for word in tokenize(message):
                        counts[word][0 if is_spam else 1] += 1
                return counts

    1. turn these counts into estimated probabilities

            def word_probabilities(counts, total_spams, total_non_spams, k=0.5):
                """turn the word_counts into a list of triplets
                w, p(w / spam) and p(w / ~spam)"""
                return [(w,
                         (spam + k) / (total_spams + 2 * k),
                         (non_spam + k) / (total_non_spams + 2 * k))
                        for w, (spam, non_spam) in counts.iteritems()]

    1. use these word probabilities (and naive bayes assumptions)

            # to assign probabilities to messages
            def spam_probability(word_probs, message):
                message_words = tokenize(message)
                log_prob_if_spam = log_prob_if_not_spam = 0.0

                # iterate through each word in our vacabulary
                for word, prob_if_spam, prob_if_not_spam in word_probs:
                    # if *word* appears in the message,
                    # add the log probability of seeing it
                    if word in message_words:
                        log_prob_if_spam += math.log(prob_if_spam)
                        log_prob_if_not_spam += math.log(prob_if_not_spam)
                    # if *word* doesn't appear in the message
                    # add the log probability of _not_ seeing it
                    # which is log(1 - probability of seeing it)
                    else:
                        log_prob_if_spam += math.log(1.0 - prob_if_spam)
                        log_prob_if_not_spam += math.log(1.0 - prob_if_not_spam)
                prob_if_spam = math.exp(log_prob_if_spam)
                prob_if_not_spam = math.exp(log_prob_if_not_spam)
                return prob_if_spam / (prob_if_spam + prob_if_not_spam)

    1. put all together into naive bayes classifier

                class NaiveBayesClassifier:

                    def __init__(self, k=0.5):
                        self.k = k
                        self.word_probs = []

                    def train(self, training_set):

                        # count spam and non-spam messages
                        num_spams = len([is_spam
                                         for message, is_spam in training_set
                                         if is_spam])

                        num_non_spams = len(training_set) - num_spams

                        word_counts = count_words(training_set)
                        self.word_probs = word_probabilities(word_counts, num_spams, num_non_spams, self.k)

                    def classifier(self, message):
                        return spam_probability(self.word_counts, message)

1. testing our model

    1. test data [spamassassin public corpus](https://spamassassin.apache.org/publiccorpus/)

    1. get subject lines of each email

            import glob, re

            path = r"C:\spam\*\*"
            data = []
            for fn in glob.glob(path):
                is_spam = 'ham' not in fn

                with open('fn', 'r') as file:
                    for line in file:
                        if line.startswith('Subject:'):
                            # remove leading 'Subject: ' and keep what's left
                            subject = re.sub(r"^Subject: ", "", line).strip()
                            data.append((subject, is_spam))

    1. split into training data and test data

            random.seed(0)
            train_data, test_data = split_data(data, 0.75)
            classifier = NaiveBayesClassifier()
            classifier.train(train_data)

    1. check how our model does

            # triplets (subject, actual is_spam, predicted spam_probability)
            classified = [(subject, id_spam, classifier.classify(subject))
                          for subject, is_spam in test_data]

            # assume that spam_probability > 0.5 corresponds to spam prediction
            # and count the combinations of (actual is_spam, predicted is_spam)
            counts = Counter((is_spam, spam_probability > 0.5)
                             for _, is_spam, spam_probability in classifier)

            # 101 true positives (spam classified => spam)
            # 33 false positives (ham  classified => spam)
            # 704 true negatives (ham  classified => ham)
            # 38 false negatives (ham  classified => spam)
            # precision => 101 / (101 + 33) = 75%
            # recall    => 101 / (101 + 38) = 73%
            # which are not bad numbers for such a simple model

            # sort by spam_probability from smallest to largest
            classified.sort(key=lambda row: row[2])

            # the highest predicted spam probabilities among the non-spams
            spammiest_hams = filter(lambda row: not row[1], classified)[-5:]

            # the lowest predicted spam probabilities among the actual spams
            hammiest_spams = filter(lambda row: row[1], classified)[:5]

            # check spammiest words
            def p_spam_given_word(word_prob):
                """uses bayes's therom to compute
                p(spam / message contains word)"""
                # word_prob is one of the triplets
                # produced by word_probabilities
                word, prob_if_spam, prob_if_not_spam = word_prob
                return prob_if_spam / (prob_if_spam / prob_if_not_spam)

            words = sorted(classifier.word_probs,
                           key=p_spam_given_word)

            spammiest_words = words[-5:]
            hammiest_words = words[:5]

    1. ways to improve this model

            # 1. look at message content, not just subject line, and take care of message headers
            
            # 2. optional min_count threshhold and ignore tokens don't appear at least that many times
            
            # 3. tokenizer has no notion of similar words (e.g. cheap cheapest), optional stemmer function
            #    e.g.
            def drop_final_s(word):
                return re.sub("s$", "", word)

            # 4. add extra features like `message contains a number`

### references

1. [porter stemmer](http://tartarus.org/martin/PorterStemmer/)