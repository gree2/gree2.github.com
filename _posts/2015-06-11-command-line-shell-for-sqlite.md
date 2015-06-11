---
layout: post
title: "command line shell for sqlite"
description: ""
category: [command]
tags: [shell, sqlite]
---
{% include JB/setup %}


### [getting started](https://www.sqlite.org/cli.html)

1. start `sqlite3` program

    1. create database

            $ sqlite3

            # create database `ex1`
            $ sqlite3 ex1
            SQLite version 3.8.4.1 2014-03-11 15:27:36
            Enter ".help" for usage hints.
            sqlite> 

    1. create table and insert data

            sqlite> create table tbl1(one varchar(10), two smallint);
            sqlite> insert into tbl1 values('hello', 10);
            sqlite> insert into tbl1 values('world', 20);
            sqlite> select * from tbl1;
            hello|10
            world|20

    1. terminate the `sqlite3` program

            sqlite> control-d

            # or
            sqlite> .exit

            # or
            sqlite> .quit

    1. save database into disk

            sqlite> .save ex1.db

    1. open database from disk

            sqlite> .epen ex1.db

    1. list of dot commands

            sqlite> .help

1. changing output format

    1. `list` mode

            sqlite> .mode list
            sqlite> select * from tbl1;
            hello|10
            world|20

    1. `list` mode separater

            sqlite> .separator ", "
            sqlite> select * from tbl1;
            hello, 10
            world, 20

    1. `line` mode

            sqlite> .mode line
            sqlite> select * from tbl1;
              one = hello
              two = 10

              one = world
              two = 20

    1. `column` mode

            sqlite> .mode column
            sqlite> select * from tbl1;
            hello       10        
            world       20        

    1. `.width` command

            sqlite> .width 12 6
            sqlite> select * from tbl1;
            hello         10    
            world         20    

    1. `.header` command

            sqlite> .header on
            sqlite> select * from tbl1;
            one           two   
            ------------  ------
            hello         10    
            world         20    

    1. insert mode

            sqlite> .mode insert new_table
            sqlite> select * from tbl1;
            INSERT INTO new_table VALUES('hello',10);
            INSERT INTO new_table VALUES('world',20);

    1. `html` mode

            sqlite> .mode html
            sqlite> select * from tbl1;
            <TR><TH>one</TH>
            <TH>two</TH>
            </TR>
            <TR><TD>hello</TD>
            <TD>10</TD>
            </TR>
            <TR><TD>world</TD>
            <TD>20</TD>
            </TR>

    1. `.explain` parsed and analyzed

            sqlite> .explain
            sqlite> explain delete from tbl1 where two<20;
            addr  opcode         p1    p2    p3    p4             p5  comment      
            ----  -------------  ----  ----  ----  -------------  --  -------------
            0     Init           0     16    0                    00               
            1     Null           0     1     0                    00               
            2     OpenRead       0     2     0     2              00               
            3     Rewind         0     9     0                    00               
            4       Column         0     1     2                    00               
            5       Ge             3     8     2     (BINARY)       6c               
            6       Rowid          0     4     0                    00               
            7       RowSetAdd      1     4     0                    00               
            8     Next           0     4     0                    01               
            9     Close          0     0     0                    00               
            10    OpenWrite      0     2     0     2              00               
            11    RowSetRead     1     15    4                    00               
            12      NotExists      0     14    4     1              00               
            13      Delete         0     1     0     tbl1           00               
            14    Goto           0     11    0                    00               
            15    Halt           0     0     0                    00               
            16    Transaction    0     1     1     0              01               
            17    TableLock      0     2     1     tbl1           00               
            18    Integer        20    3     0                    00               
            19    Goto           0     1     0                    00               

1. write results to a file

            sqlite> .mode list
            sqlite> .separator |
            sqlite> .output tbl1.txt
            sqlite> select * from tbl1;
            sqlite> .exit
            
            $ cat tbl1.txt 
            one|two
            hello|10
            world|20

1. file i/o functions

            # read content from a file
            # into an table column
            sqlite> create table images(name text, type text, img blob);
            sqlite> insert into images(name, type, img)
               ...>   values('icon', 'jpeg', readfile('icon.jpg'));

            # write content of a column
            # into a file
            sqlite> select writefile('icon.jpg', imp)
               ...>   from images where name='icon';

1. querying database schema

    1. list all databases

            sqlite> .databases
            sqlite> .databases
            seq  name             file                                                      
            ---  ---------------  ----------------------------------------------------------
            0    main             /Users/hqlgree2/Documents/hobby/sqlite/ex1.db             
            1    temp                                                                       

    1. list all tables

            sqlite> .tables

    1. create `table` or create `index` statements

            sqlite> .schema
            CREATE TABLE tbl1(one varchar(10), two smallint);
            sqlite> .schema tbl1 
            CREATE TABLE tbl1(one varchar(10), two smallint);

1. csv import and export

    1. import

            sqlite> .mode csv
            sqlite> .import /path/to/data.csv tbl1;

    1. export

            sqlite> .header on
            sqlite> .mode csv
            # causes all query output
            # to go into the named file
            sqlite> .once /path/to/dataout.csv
            sqlite> select * from tbl1;
            sqlite> .system /path/to/dataout.csv

1. convert an entire database to an ascii text file

            $ echo '.dump' | sqlite3 ex1 | gzip -c >ex1.dump.gz

            # reconstruct the database
            $ zcat ex1.dump.gz | sqlite3 ex2

            # export an sqlite database into other sql database
            $ createdb ex2
            $ sqlite3 ex1 .dump | psql ex2