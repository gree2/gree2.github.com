---
layout: post
title: "tool mdbtools read access databases"
description: ""
category: [tool]
tags: [mdbtools, access database]
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

### [reference](http://nialldonegan.me/2007/03/10/converting-microsoft-access-mdb-into-csv-or-mysql-in-linux/)