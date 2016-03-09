---
layout: post
title: "command join"
description: ""
category: [command]
tags: [join]
---
{% include JB/setup %}


### [info](http://www.computerhope.com/unix/ujoin.htm)

1. `join` the lines of two files which share a common field of data

1. default join field is the first

1. delimited by whitespace

1. `file1` and `file2` must be sorted on the join fields

### options

1. `-tchar` use char as input and output field separator

1. `-1 field` join on this `field` of file 1

1. `-2 field` join on this `field` of file 2

### demo

1. [computerhope demo](http://www.computerhope.com/unix/ujoin.htm)

    1. test data

            $ pico file1.txt
            1 India
            2 US
            3 Ireland
            4 UK
            5 Canada

            $ pico file2.txt
            1 NewDelhi
            2 Washington
            3 Dublin
            4 London
            5 Toronto

    1. join the contents

            $ join file1.txt file2.txt
            1 India NewDelhi
            2 US Washington
            3 Ireland Dublin
            4 UK London
            5 Canada Toronto

    1. redirect to a file

            $ join file1.txt file2.txt > file3.txt

1. [albany edu demo](http://www.albany.edu/~ig4895/join.htm)

    1. test data

            $ pico file1.txt
            1 abc
            2 lmn
            3 pqr

            $ pico file2.txt
            1 abc
            3 lmn
            9 opq

    1. join with 1st column

            $ join file1.txt file2.txt
            1 abc abc
            3 pqr lmn

    1. join with 2ed column `inner join`

            $ join -1 2 -2 2 file1.txt file2.txt

    1. include all records from 1st file `left outer join`

            $ join -a1 -1 2 -2 2 file1.txt file2.txt
            abc 1 1
            lmn 2 3
            pqr 3

    1. also include all records from 2ed file `full outer join`

            $ join -a1 -a2 -1 2 -2 2 file1.txt file2.txt
            abc 1 1
            lmn 2 3
            opq 9
            pqr 3

    1. format the output

            # `-o` option
            # `0` display the join field
            
            # `x.y` display `y` field from the `x` file
            # `2.1` means display 1st field from 2ed file

            # `-e "0"` replace missing data fields
            # with whatever is in-between the quotes
            # in this case a `0`
            $ join -a1 -a2 -1 2 -2 2 -o 0 1.1 2.1 -e "X" file1.txt file2.txt
            abc 1 1
            lmn 2 3
            opq X 9
            pqr 3 X