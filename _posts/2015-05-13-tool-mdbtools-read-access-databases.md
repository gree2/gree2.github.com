---
layout: post
title: "tool mdbtools read access databases"
description: ""
category: [tool]
tags: [mdbtools, access, schema, mysql, oracle, postgres, dump, csv]
---
{% include JB/setup %}


### install

1. install mdbtools

            $ brew install mdbtools

1. dependencies

    1. autoconf
    
    1. automake
    
    1. glib

    1. libtool
    
    1. pkg-config
    
    1. readline
    
    1. txt2man

### convert mdb to csv

1. get list of tables

            $ mdb-tables database.mdb

1. get a csv version for each table

            $ mdb-export database.mdb tablename

### convert mdb to a format required by mysql

1. get the table schema into database

            $ mdb-schema database.mdb | mysql -u username -p mysql_database

1. import each table by running


            $ mdb-export -I database.mdb tablename | sed -e 's/)$/)\;/' | mysql -u username -p mysql_database

### get schema into mysql

1. create database

            $ mysqladmin create mysql_database

1. export

            $ mdb-schema database.mdb postgres \
            | sed 's/Int8/int'                 \
            | sed 's/Char /varchar/'           \
            | sed 's/^-/#/'                    \
            | grep -v '^DROP'                  \
            | mysql mysql_database

            # 1st sed turns Int8 into int
            # 2ed sed turns Char into varchar
            # 3rd sed turns postgres comments into mysql comments

            # before the CREATE TABLE statement
            # there is a DROP TABLE statement 
            # grep drops the DROP statement

### generate proper ddl output form mdb-schema

* you should specify the target dialect on the command line

    * other dialects supported are `access`, `sybase`, `oracle`, `postgres`

            $ mdb-schema database.mdb mysql | mysql -u username -p mysql_database

* [reference](http://nialldonegan.me/2007/03/10/converting-microsoft-access-mdb-into-csv-or-mysql-in-linux/)

### access dump to csvs

* [reference](http://mazamascience.com/WorkingWithData/?p=168)

* code

            #!/usr/bin/env python
            """
            access_dump_csv.py
                A simple script to dump the contents of a Microsoft Access Database.
                It depends upon the mdbtools suite:
                http://sourceforge.net/projects/mdbtools/
            """

            # the subprocess module is new in python v 2.4
            import sys, subprocess

            DATABASE = sys.argv[1]

            # Get the list of table names with 'mdb-tables'
            TABLENAMES = subprocess.Popen(['mdb-tables', '-1', DATABASE], stdout=subprocess.PIPE).communicate()[0]

            # Dump each table as a CSV file using 'mdb-export',
            # converting ' ' in table names to '_' for the CSV filenames.
            for tablename in TABLENAMES.split('\n'):
                if tablename == '':
                    continue
                filename = tablename.replace(' ', '_').lower() + '.csv'
                with open(filename, 'w') as f:
                    print 'dumping ' + tablename
                    contents = subprocess.Popen(['mdb-export', DATABASE, tablename], stdout=subprocess.PIPE).communicate()[0]
                    f.write(contents)

* usage

            $ python access_dump_csv.py database.mdb