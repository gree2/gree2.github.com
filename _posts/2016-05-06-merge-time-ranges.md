---
layout: post
title: "merge time ranges"
description: ""
category: [python]
tags: [merge, time, range]
---
{% include JB/setup %}


### from stackoverflow

1. [merging a list of time range tuples that have overlapping time ranges](http://stackoverflow.com/questions/5679638/merging-a-list-of-time-range-tuples-that-have-overlapping-time-ranges)

    1. code original

            [(1, 5), (2, 4), (3, 6)] --->  [(1,6)]
            [(1, 3), (2, 4), (5, 8)] --->  [(1, 4), (5,8)]

            # Algorithm
            # initialranges: [(a,b), (c,d), (e,f), ...]
            # First we sort each tuple then whole list.
            # This will ensure that a<b, c<d, e<f ... and a < c < e ... 
            # BUT the order of b, d, f ... is still random
            # Now we have only 3 possibilities
            #================================================
            # b<c<d: a-------b           Ans: [(a,b),(c,d)]
            #                  c---d
            # c<=b<d: a-------b          Ans: [(a,d)]
            #               c---d
            # c<d<b: a-------b           Ans: [(a,b)]
            #         c---d
            #================================================
            def mergeoverlapping(initialranges):
                i = sorted(set([tuple(sorted(x)) for x in initialranges]))

                # initialize final ranges to [(a,b)]
                f = [i[0]]
                for c, d in i[1:]:
                    a, b = f[-1]
                    if c<=b<d:
                        f[-1] = a, d
                    elif b<c<d:
                        f.append((c,d))
                    else:
                        # else case included for clarity. Since 
                        # we already sorted the tuples and the list
                        # only remaining possibility is c<d<b
                        # in which case we can silently pass
                        pass
                return f

    1. pythonic

            1. eliminate the set() construction, since the algorithm should prune out duplicates during in the main loop
            
            2. if you just need to iterate over the results, use yield to generate the values
            
            3. reduce construction of intermediate objects
                e.g. move the tuple() call to the point where the final values are produced
                saving you from having to construct and throw away extra tuples
                and reuse a list saved for storing the current time range for comparison

            def merge(times):
                saved = list(times[0])
                for st, en in sorted([sorted(t) for t in times]):
                    if st <= saved[1]:
                        saved[1] = max(saved[1], en)
                    else:
                        yield tuple(saved)
                        saved[0] = st
                        saved[1] = en
                yield tuple(saved)

            data = [
                [(1, 5), (2, 4), (3, 6)],
                [(1, 3), (2, 4), (5, 8)]
                ]

            for times in data:
                print list(merge(times))

    1. for dict

            # 1. code
            def merge_range(ranges: list, start_key: str, end_key: str):
                ranges = sorted(ranges, key=lambda x: x[start_key])
                saved = dict(ranges[0])

                for range_set in sorted(ranges, key=lambda x: x[start_key]):
                    if range_set[start_key] <= saved[end_key]:
                        saved[end_key] = max(saved[end_key], range_set[end_key])
                    else:
                        yield dict(saved)
                        saved[start_key] = range_set[start_key]
                        saved[end_key] = range_set[end_key]
                yield dict(saved)

            # 2. data
            data = [
                {'start_time': '09:00:00', 'end_time': '11:30:00'},
                {'start_time': '15:00:00', 'end_time': '15:30:00'},
                {'start_time': '11:00:00', 'end_time': '14:30:00'},
                {'start_time': '09:30:00', 'end_time': '14:00:00'}
            ]

            # 3. execution
            print(list(merge_range(ranges=data, start_key='start_time', end_key='end_time')))

            # 4. output
            [
                {'start_time': '09:00:00', 'end_time': '14:30:00'},
                {'start_time': '15:00:00', 'end_time': '15:30:00'}
            ]

1. my use case (dict)

            def to_date(date_str):
                """to_date
                convert date_str to datetime"""
                return datetime.strptime(date_str, '%Y-%m-%d %H:%M:%S.%f')


            def mapper(row):
                """mapper
                convert timedelta to minutes"""
                dt1 = to_date(row[start_key])
                dt2 = to_date(row[end_key])
                return (dt2 - dt1).seconds / 60


            def merge_range(ranges, start_key, end_key):
                """merge_range
                merge timerange that might overlapping"""
                ranges = sorted(ranges, key=lambda x: x[start_key])
                saved = dict(ranges[0])

                for range_set in sorted(ranges, key=lambda x: x[start_key]):
                    if range_set[start_key] <= saved[end_key]:
                        saved[end_key] = max(saved[end_key], range_set[end_key])
                    else:
                        yield dict(saved)
                        saved[start_key] = range_set[start_key]
                        saved[end_key] = range_set[end_key]
                yield dict(saved)


            def process_data():
                """process_data
                process *.csv file"""
                df = pd.read_csv(file_in)

                sio = StringIO.StringIO()
                for name_date, group_date in df.groupby(column_date):
                    for name_man, group_man in group_date.groupby(column_man_id):
                        data_time = group_man[[start_key, end_key]]
                        data_json = json.loads(data_time.to_json(orient='records'))
                        time_range = list(merge_range(data_json, start_key, end_key))
                        time_delta = map(mapper, time_range)
                        timing = reduce(lambda x, y: x + y, time_delta)
                        timing_data = '{},{},{}\n'.format(name_date, name_man, timing)
                        sio.write(timing_data)
                with open(file_ou, 'w') as f:
                    f.write(sio.getvalue())

            process_data()