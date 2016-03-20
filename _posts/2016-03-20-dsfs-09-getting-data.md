---
layout: post
title: "dsfs 09 getting data"
description: ""
category: [python, data science]
tags: [python, data science, data, command line]
---
{% include JB/setup %}


### stdin and stdout

1. pipe data

    1. code

            # egrep.py
            import sys, re

            # sys.argv is the list of command-line arguments
            # sys.argv[0] is the name of the program it self
            # sys.argv[1] will be the regex specified at the command line
            regex = sys.argv[1]

            # for every line passed into the script
            for line in sys.stdin:
                # if it matched the regex, write it to stdout
                if re.search(regex, line):
                    sys.stdout.write(line)

    1. line count

            # line_count.py
            import sys

            count = 0
            for line in sys.stdin:
                count += 1

            # print goes to sys.stdout
            print count

    1. run

            # on windows
            > type demo.txt | python egrep.py "[0-9]" | python line_count.py

            # unix
            > cat demo.txt | python egrep.py "[0-9]" | python line_count.py

1. word count

    1. code

            # most_common_words.py
            import sys
            from collections import Counter

            # pass in number of words at first argument
            try:
                num_words = int(sys.argv[1])
            except:
                print "usage: most_common_words.py num_words"
                # non-zero exit code indicates error
                sys.exit(1)

            counter = Counter(word.lower()
                              for line in sys.stdin
                              for word in line.strip().split()
                              if word)

            for word, count in counter.most_common(num_words):
                sys.stdout.write(str(count))
                sys.stdout.write('\t')
                sys.stdout.write(word)
                sys.stdout.write('\n')

    1. run

            > type the_bible.txt | python most_common_words.py 10

### reading files

1. text files

    1. code

            # r w a readonly write append
            reading = open('demo.txt', 'r')
            writing = open('demo.txt', 'w')
            appending = open('demo.txt', 'a')

            reading.close()

    1. use `with`

            with open('demo.txt', 'r') as f:
                data = func_gets_data_from(f)
            process(data)

    1. iterate

            starts_with_hash = 0
            with open('demo.txt', 'r') as f:
                for line in f:
                    if re.match("^#", line):
                        starts_with_hash += 1

### delimited files

1. tab-separated comma-separated

    1. code

            import csv
            with open('tab.txt', 'rb') as f:
                reader = csv.reader(f, delimiter='\t')
                for row in reader:
                    date = row[0]
                    symbol = row[1]
                    price = float(row[2])
                    process(date, symbol, price)

    1. skip header row

            with open('tab.txt', 'rb') as f:
                reader = csv.DictReader(f, delimter=':')
                for row in reader:
                    date = row['date']
                    symbol = row['symbol']
                    price = row['price']
                    process(date, symbol, price)

    1. write

            today_prices = { 'AAPL': 90.91, 'MSFT': 41.68, 'FB': 64.5 }

            with open('demo.txt', 'wb') as f:
                writer = csv.writer(f, delimiter=',')
                for stock, price in today_prices.items():
                    writer.writerow([stock, price])

### scraping the web

1. html and parsing thereof

    1. html parser

            # python's built-in html parser is not that lenient
            # install one
            $ pip install

            from bs4 import BeautifulSoup
            import requests

            html = requests.get('http://www.example.com').text
            soup = BeautifulSoup(html, 'html5lib')

            # find first <p> tag
            first_paragraph = soup.find('p')
            # or
            first_paragraph = soup.p

            # `text` property
            first_paragraph_text = soup.p.text