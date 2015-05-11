---
layout: post
title: "pandas sqlalchemy pyodbc query sql server and plotting"
description: ""
category: [python]
tags: [pandas, sqlalchemy, pyodbc, sql server, plotting]
---
{% include JB/setup %}


### import

1. inline backend

            %matplotlib inline
            import matplotlib
            matplotlib.style.use('ggplot')

1. use font `Microsoft YaHei` check my post [2015-04-27-python-change-matplotlib-font-on-mac]({% post_url 2015-04-27-python-change-matplotlib-font-on-mac %})

            matplotlib.rcParams['font.sans-serif'] = ['Microsoft YaHei']
            matplotlib.rcParams['axes.unicode_minus'] = False
            import pandas.io.sql
            import pyodbc

### connection string

1. driver and ...

            driver   = '{SQL Server}'
            server   = '10.12.11.34'
            database = 'database'
            uid      = 'user'
            pwd      = 'pass'

### connect and read

1. unicode and chinese

            conn_string = 'DRIVER={};SERVER={};DATABASE={};UID={};PWD={};' \
                .format(driver, server, database, uid, pwd)
            conn = pyodbc.connect(conn_string, unicode_results=True, collation='Chinese_PRC_CI_AS')

1. read data into dataframe

            df = pandas.io.sql.read_sql(sql, conn)
            df.head()

### rename columns names

1. rename

            df.columns = [u'name1',
            u'name2',
            u'name3',
            u'name4',
            u'name5',
            u'name6',
            u'name7',
            u'name8',
            u'name9',
            u'name0']

1. print column name

            for name in df.columns.values.tolist():
                print name

### play with data

1. show top five lines

            df.head()

1. filter data

            unit_001001 = df[df[u'name1']=='001001']
            unit_001001.head()

1. select data (all rows and column 3, 7, 8)

            unit_001001 = unit_001001.ix[:, [3, 7, 8]]
            unit_001001.head()

1. sort and index column

            unit_001001 = unit_001001.sort([u'name3', u'name7'], ascending=[1, 1])
            unit_001001 = unit_001001.set_index(u'name3')
            unit_001001.head()

1. `bar` plotting

            unit_001001 = unit_001001.sort(u'name7', ascending=True)
            unit_001001.plot(kind='bar', figsize=(15, 5), fontsize=12);

1. `barh` plotting

            unit_001001.plot(kind='barh', figsize=(5, 15), fontsize=12);

1. other plotting

            area    => area plot
            bar     => vertical bar plot
            barh    => horizontal bar plot
            box     => boxplot
            density => same as kde
            hexbin  => hexbin plot
            hist    => histogram
            kde     => Kernel Density Estimation plot
            line    => line plot                       <= default
            pie     => pie plot
            scatter => scatter plot