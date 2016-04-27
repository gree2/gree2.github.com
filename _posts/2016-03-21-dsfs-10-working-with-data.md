0---
layout: post
title: "dsfs 10 working with data"
description: ""
category: [python, data science]
tags: [python, data science, data]
---
{% include JB/setup %}


### exploring your data

1. 1d

    1. summary statistics

            # count, min, max, mean, standard deviation
            # histogram, discrete buckets

            def bucketize(point, bucket_size):
                """floor the point to the next lower multiple of bucket_size"""
                return bucket_size * math.floor(point / bucket_size)

            def make_histogram(points, bucket_size):
                """buckets the points and counts how many in each bucket"""
                return Counter(bucketize(point, bucket_size) for point in points)

            def plot_histogram(points, bucket_size, title=''):
                histogram = make_histogram(points, bucket_size)
                plt.bar(histogram.keys(), histogram.values(), width=bucket_size)
                plt.title(title)
                plt.show()

    1. demo

            random.seed(0)

            # uniform between -100 and 100
            uniform = [200 * random.random() -100 for _ in range(10000)]

            # normal distribution with mean 0, standard deviation 57
            normal = [57 * inverse_normal_cdf(random.random())
                      for _ in range(10000)]

            # both have means close to 0
            # and standard deviation close to 58
            # however they have very different distribution

            plot_histogram(uniform, 10, 'uniform histogram')
            plot_histogram(normal, 10, 'normal histogram')

1. 2d

    1. fake data set

            def random_normal():
                """returns a random draw from a standard normal distribution"""
                return inverse_normal_cdf(random.random())

            xs = [random_normal() for _ in range(1000)]
            ys1 = [x + random_normal() / 2 for x in xs]
            ys1 = [-x + random_normal() / 2 for x in xs]
            plt.scatter(xs, ys1, marker='.', color='black', label='ys1')
            plt.scatter(xs, ys2, marker='.', color='gray', label='ys2')
            plt.xlabel('xs')
            plt.ylabel('ys')
            plt.legend(loc=9)
            plt.title('very different joint distribution')
            plt.show()

    1. correlations

            # 0.9 -0.9
            print correlations(xs, ys1)
            print correlations(xs, ys2)

1. many dimensions

    1. correlation matrix

            # how all dimensions relate to one another
            def correlation_matrix(data):
                """returns the num_column x num_columns matrix whose (i, j)th entry
                is the correlation between columns i and j of data"""
                _, num_columns = shape(data)

                def matrix_entry(i, j):
                    return correlation(get_column(data, i), get_column(data, j))

                return make_matrix(num_columns, num_columns, matrix_entry)

    1. scatter-plot matrix

            import matplotlib.pyplot as plt

            _, num_columns = shape(data)
            fig, ax = plt.subplots(num_columns, num_columns)

            for i in range(num_columns):
                for j in range(num_columns):
                    if i != j:
                        ax[i][j].scatter(get_column(data, j), get_column(data, i))
                    else:
                        ax[i][j].annotate('series' + str(i), (0.5, 0.5),
                                          xycoords='axes fraction',
                                          ha='center', va='center')

                    # hide axis labels except left and bottom charts
                    if i < num_columns - 1:
                        ax[i][j].xaxis.set_visible(False)
                    if j > 0:
                        ax[i][j].yaxis.set_visible(False)

            # fix the bottom right and top left axis labels
            # which are wrong because their charts only have text in them
            ax[-1][-1].set_xlim(ax[0][-1].get_xlim())
            ax[0][0].set_ylim(ax[0][1].get_ylim())
            plt.show()

### cleaning and munging

1. real world data is dirty

    1. parsing

            def parse_row(input_row, parsers):
                """given a list of parsers (some of which may be Noe)
                apply the appropriate one to each element of the input_row"""
                return [parser(value) if parser is not None else value
                        for value, parser in zip(input_row, parsers)]

            def parse_row_with(reader, parsers):
                """wrap a reader to apply the parsers to each of its rows"""
                for row in reader:
                    yield parse_row(row, parsers)

    1. get None rather than crash

            def try_or_noe(f):
                """wraps f to return None if f raises an exception
                assume f takes only one input"""
                def f_or_none(x):
                    try: return f(x)
                    except: return None
                return f_or_none

            def parse_row(input_row, parsers):
                """given a list of parsers (some of which may be Noe)
                apply the appropriate one to each element of the input_row"""
                return [try_or_none(parser)(value) if parser is not None else value
                        for value, parser in zip(input_row, parsers)]

    1. test

            6/20/2014,AAPL,90.91
            6/20/2014,MSFT,41.68
            6/20/3014,FB,64.5
            6/19/2014,AAPL,91.86
            6/19/2014,MSFT,n/a
            6/19/2014,FB,64.34

            import dateutil.parser
            data = []

            with open('stock_prices.csv', 'rb') as f:
                reader = csv.reader(f)
                for line in parse_rows_with(reader, [dateutil.parser.parse, None, float]):
                    data.append(line)

            # check for None rows
            for row in data:
                if any(x is None for x in row):
                    print row

    1. dict of parsers by field name

            def try_parse_field(field_name, value, parser_dict):
                """try to parse value using the appropriate function from parser_dict"""
                parser = parser_dict.get(field_name)
                if parser is not None:
                    return try_or_none(parser)(value)
                else:
                    return value

            def parse_dict(input_dict, parser_dict):
                return { field_name: try_parse_field(field_name, value, parser_dict)
                         for field_name, value in input_dict.iteritmes() }

### manipulating data

1. 