---
layout: post
title: "python csvkit"
description: ""
category: [command, python]
tags: [csv, csvkit, csvclean, csvcut, csvformat, csvgrep, csvjoin, csvjson, csvlook, csvpy, csvsort, csvsql, csvstack, csvstat, in2csv, sql2csv]
---
{% include JB/setup %}


### info

1. csvkit is a suite of command-line tools

			for converting to and workding with csv

1. [github repo](https://github.com/wireservice/csvkit)

1. [document](http://csvkit.readthedocs.org/en/540/)

### why csvkit

1. convert

	1. excel to csv

			$ in2csv data.xls > data.csv

	1. json to csv

			$ in2csv data.json > data.csv

	1. csv to json

			$ csvjson data.csv > data.json

	1. postgresql

			# import into postgresql
			csvsql --db postgresql:///db --insert data.csv

			# export from postgresql
			sql2csv --db postgresql:///db --query "select * from data" > data.csv

1. print

	1. column names

			$ csvcut -n data.csv

	1. select subset of columns

			$ csvcut -c col_a,col_c data.csv > new.csv

	1. reorder columns

			$ csvcut -c col_c,col_a data.csv > new.csv

1. find

	1. rows matching cells

			$ csvgrep -c phone_number -r "555-555-\d{4}" data.csv > matching.csv

1. summary statistics

			$ csvstat data.csv

1. query with sql

			$ csvsql --query "select name from data where age > 30" data.csv > old_folks.csv

### usage

1. [input](http://csvkit.readthedocs.org/en/540/scripts/cli.html#input)

	1. [in2csv](http://csvkit.readthedocs.org/en/540/scripts/in2csv.html)

	1. [sql2csv](http://csvkit.readthedocs.org/en/540/scripts/sql2csv.html)

1. [processing](http://csvkit.readthedocs.org/en/540/scripts/cli.html#processing)

	1. [csvclean](http://csvkit.readthedocs.org/en/540/scripts/csvclean.html)

	1. [csvcut](http://csvkit.readthedocs.org/en/540/scripts/csvcut.html)

	1. [csvgrep](http://csvkit.readthedocs.org/en/540/scripts/csvgrep.html)

	1. [csvjoin](http://csvkit.readthedocs.org/en/540/scripts/csvjoin.html)

	1. [csvsort](http://csvkit.readthedocs.org/en/540/scripts/csvsort.html)

	1. [csvstack](http://csvkit.readthedocs.org/en/540/scripts/csvstack.html)

1. [output and analysis](http://csvkit.readthedocs.org/en/540/scripts/cli.html#output-and-analysis)

	1. [csvformat](http://csvkit.readthedocs.org/en/540/scripts/csvformat.html)

	1. [csvjson](http://csvkit.readthedocs.org/en/540/scripts/csvjson.html)

	1. [csvlook](http://csvkit.readthedocs.org/en/540/scripts/csvlook.html)

	1. [csvpy](http://csvkit.readthedocs.org/en/540/scripts/csvpy.html)

	1. [csvsql](http://csvkit.readthedocs.org/en/540/scripts/csvsql.html)

	1. [csvstat](http://csvkit.readthedocs.org/en/540/scripts/csvstat.html)
