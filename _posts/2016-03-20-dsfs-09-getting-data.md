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
            first_paragraph_words = soup.p.text.split()

            # tag's attributes => dict
            first_paragraph_id = soup.p['id']       # raises KeyError if no 'id'
            first_paragraph_id2 = soup.p.get('id')  # returns None if no 'id'
            
            # get multiple tags at once
            all_paragraphs = soup.find_all('p')   # or just soup('p')
            paragraphs_with_ids = [p for p in soup('p') if p.get('id')]
            
            # find tags with a specific class
            important_paragraphs = soup('p', {'class' : 'important'})
            important_paragraphs2 = soup('p', 'important')
            important_paragraphs3 = [p for p in soup('p')
            if 'important' in p.get('class', [])]

            #
            spans_inside_divs = [span
                                 for div in soup('div')    # for each <div> on the page
                                 for span in div('span')]  # find each <span> inside it

### e.g. o'reilly books about data

1. e.g.

    1. url

            http://shop.oreilly.com/category/browse-subjects/data.do?sortby=publicationDate&page=1

    1. robots.txt

            craw-delay: 30
            request-rate: 1/30

            # 1. wait 30 seconds between requests
            # 2. request only one page every 30 seconds

    1. code

            url = "http://shop.oreilly.com/category/browse-subjects/data.do?sortby=publicationDate&page=1"
            soup = BeautifulSoup(requests.get(url).text, 'html5lib')
            tds = soup('td', 'thumbtext')
            print len(tds)   # 30

            def is_video(td):
                """it's a video if it has exactly one pricelabel
                and if the stripped text inside that pricelabel starts with 'video'"""
                pricelabels = td('span', pricelabel)
                return (len(pricelabels) == 1 and pricelabels[0].text.strip().startswith('Video'))

            print len([td for td in tds if not is_video(td)])   # 21

            # title author isbn date
            def book_info(td):
                """given a `td` tag representing a book
                extract the book's details and return as dict"""
                title = td.find('div', 'thumbheader').a.text
                by_author = td.find('div', 'AuthorName').text
                authors = [x.strip() for x in re.sub('^By ', "", by_author).split(',')]
                isbn_link = td.find('div', 'thumbheader').a.get('href')
                isbn = re.match('product/(.*)\.do', isbn_link).groups()[0]
                date = td.find('span', 'directorydate').text.strip()
                return {
                    "title" : title,
                    "authors" : authors,
                    "isbn" : isbn,
                    "date" : date
                }

    1. scrape

            from bs4 import BeautifulSoup
            import requests
            from time import sleep
            base_url = "http://shop.oreilly.com/category/browse-subjects/data.do?sortby=publicationDate&page="
            books = []
            NUM_PAGES = 31 # at the time of writing, probably more by now
            for page_num in range(1, NUM_PAGES + 1):
                print "souping page", page_num, ",", len(books), " found so far"
                url = base_url + str(page_num)
                soup = BeautifulSoup(requests.get(url).text, 'html5lib')
                for td in soup('td', 'thumbtext'):
                    if not is_video(td):
                        books.append(book_info(td))
                # now be a good citizen and respect the robots.txt!
                sleep(30)

    1. plot

            def get_year(book):
                """book["date"] looks like 'November 2014' so we need to
                split on the space and then take the second piece"""
                return int(book["date"].split()[1])

            # 2014 is the last complete year of data (when I ran this)
            year_counts = Counter(get_year(book) for book in books
                                  if get_year(book) <= 2014)

            import matplotlib.pyplot as plt
            years = sorted(year_counts)
            book_counts = [year_counts[year] for year in years]
            plt.plot(years, book_counts)
            plt.ylabel("# of data books")
            plt.title("Data is Big!")
            plt.show()

### using apis

1. json (and xml)

    1. code

            import json
            serialized = """{ "title" : "Data Science Book",
                              "author" : "Joel Grus",
                              "publicationYear" : 2014,
                              "topics" : [ "data", "science", "data science"] }"""

            # parse the JSON to create a Python dict
            deserialized = json.loads(serialized)
            if "data science" in deserialized["topics"]:
            print deserialized

1. using an unauthenticated api

    1. code

            import requests, json
            endpoint = "https://api.github.com/users/joelgrus/repos"
            repos = json.loads(requests.get(endpoint).text)

    1. python-dateutil

            from dateutil.parser import parse

            dates = [parse(repo["created_at"]) for repo in repos]
            month_counts = Counter(date.month for date in dates)
            weekday_counts = Counter(date.weekday() for date in dates)

            last_5_repositories = sorted(repos,
                                         key=lambda r: r["created_at"],
                                         reverse=True)[:5]
            last_5_languages = [repo["language"]
                                for repo in last_5_repositories]

1. finding apis

1. e.g. using the twitter apis

    1. getting credentials

    1. using twython

            from twython import Twython

            twitter = Twython(CONSUMER_KEY, CONSUMER_SECRET)
            # search for tweets containing the phrase "data science"
            for status in twitter.search(q='"data science"')["statuses"]:
                user = status["user"]["screen_name"].encode('utf-8')
                text = status["text"].encode('utf-8')
                print user, ":", text
                print

    1. streaming api

            from twython import TwythonStreamer

            # appending data to a global variable is pretty poor form
            # but it makes the example much simpler
            tweets = []

            class MyStreamer(TwythonStreamer):
                """our own subclass of TwythonStreamer that specifies
                how to interact with the stream"""

                def on_success(self, data):
                    """what do we do when twitter sends us data?
                    here data will be a Python dict representing a tweet"""
                    # only want to collect English-language tweets
                    if data['lang'] == 'en':
                        tweets.append(data)
                        print "received tweet #", len(tweets)
                    # stop when we've collected enough
                    if len(tweets) >= 1000:
                        self.disconnect()

                def on_error(self, status_code, data):
                    print status_code, data
                    self.disconnect()

            stream = MyStreamer(CONSUMER_KEY, CONSUMER_SECRET, ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
            # starts consuming public statuses that contain the keyword 'data'
            stream.statuses.filter(track='data')
            # if instead we wanted to start consuming a sample of *all* public statuses
            # stream.statuses.sample()
            top_hashtags = Counter(hashtag['text'].lower()
                                   for tweet in tweets
                                   for hashtag in tweet["entities"]["hashtags"])
            print top_hashtags.most_common(5)

### reference

1. [scrapy](http://scrapy.org/)