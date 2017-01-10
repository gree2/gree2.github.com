---
layout: post
title: "pandas date_range"
description: ""
category: [python]
tags: [pandas, date, range]
---
{% include JB/setup %}


### demo

1. code

            #! coding: utf-8

            import pandas as pd
            from datetime import datetime, time

            def dt_desc(dt):
                """dt_desc"""
                print dir(dt)

            rng = pd.date_range('2016-01-01', '2017-01-01', freq='D')
            df = pd.DataFrame(rng, columns=['dt'], index=rng)
            df['dt_str'] = df['dt'].apply(dt_desc)
            df['dt_str'] = df['dt'].apply(lambda dt: dt.strftime("%Y-%m-%d"))

            df.to_csv('demo.csv', index=False, encoding='utf-8')

1. apply(dt_desc)

            '__add__', '__class__', '__delattr__', '__dict__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__pyx_vtable__', '__qualname__', '__radd__', '__reduce__', '__reduce_ex__', '__repr__', '__rsub__', '__setattr__', '__setstate__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__weakref__', '_date_repr', '_get_field', '_get_start_end_field', '_has_time_component', '_repr_base', '_time_repr',

            'asm8', 'astimezone', 'combine', 'ctime', 'date',
            'day', 'dayofweek', 'dayofyear', 'days_in_month', 'daysinmonth',
            'dst', 'freq', 'freqstr', 'fromordinal', 'fromtimestamp',
            'hour', 'is_month_end', 'is_month_start', 'is_quarter_end', 'is_quarter_start',
            'is_year_end', 'is_year_start', 'isocalendar', 'isoformat', 'isoweekday',
            'max', 'microsecond', 'min', 'minute', 'month',
            'nanosecond', 'normalize', 'now', 'offset', 'quarter',
            'replace', 'resolution', 'second', 'strftime', 'strptime',
            'time', 'timetuple', 'timetz', 'to_datetime', 'to_datetime64',
            'to_julian_date', 'to_period', 'to_pydatetime', 'today', 'toordinal',
            'tz', 'tz_convert', 'tz_localize', 'tzinfo', 'tzname',
            'utcfromtimestamp', 'utcnow', 'utcoffset', 'utctimetuple', 'value',
            'week', 'weekday', 'weekofyear', 'year'

1. format strings

            %%
            %a %A %b %B %c %C %d %D %e %F %g %G %h %H %I %j %m %M
            %n %p %r %R %S %t %T %u %U %V %w %W %x %X %y %Y %z %Z

            %a Mon
            %A Monday
            %b Mar
            %B March
            %c 03/07/16 18:40:16
            %d 07
            %H 18
            %I 06
            %j 067
            %m 03
            %M 40
            %p PM
            %S 16
            %U 10
            %w 1
            %W 10
            %x 03/07/16
            %X 18:40:16
            %y 16
            %Y 2016
            %z 
            %Z 