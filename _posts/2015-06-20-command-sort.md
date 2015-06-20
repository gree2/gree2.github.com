---
layout: post
title: "command sort"
description: ""
category: [command]
tags: [sort]
---
{% include JB/setup %}


### [info](http://www.computerhope.com/unix/usort.htm)

1. `sort` sorts the contents of a text file, line by line

1. rules

    1. number before letter

    1. alphabet

    1. lowercase before uppercase

### demo

1. simple sort

    1. test data

            $ cat data.txt
            apples
            oranges
            pears
            kiwis
            bananas

    1. sort lines in this file alphabetically

            $ sort data.txt

    1. `redirect` output

            $ sort data.txt > output.txt

            # same as
            $ sort -o output.txt data.txt

    1. sorting in reverse order

            # using `-r` flag
            $ sort -r data.txt

1. handling mixed-case data

    1. test data

            $ cat data.txt
            a
            b
            A
            B
            b
            c
            D
            d
            C

    1. simple sort

            $ sort data.txt
            a
            A
            b
            b
            B
            c
            C
            d
            D

    1. enforce bytewise sorting

            $ export LC_ALL=C
            $ sort data.txt
            A
            B
            C
            D
            a
            b
            b
            c
            d

            # performing a `case-insensitive bytewise` sort
            $ sort -f data.txt
            A
            a
            B
            b
            b
            C
            c
            D
            d

1. checking for sorted order

            $ sort -c data.txt

1. sorting multiple files using the output of `find`

            $ find . -name "data?.txt" -print0 | sort --files0-from=-

1. comparing only selected fields of data

    1. based on names

            # 1. test data
            $ cat data.txt
            01 Joe
            02 Marie
            03 Albert
            04 Dave

            # 2. simple sort
            $ sort data.txt
            01 Joe
            02 Marie
            03 Albert
            04 Dave

            # 3. `k` stands for `key`
            # -k pos1,pos2
            $ sort -k 2 data.txt

    1. ignore and specify where to stop

            # 1. test data
            $ cat data.txt
            01 Joe Sr.Designer
            02 Marie Jr.Developer
            03 Albert Jr.Designer
            04 Dave Sr.Developer

            # 2. sort by field 3
            $ sort -k 3 data.txt
            03 Albert Jr.Designer
            02 Marie Jr.Developer
            01 Joe Sr.Designer
            04 Dave Sr.Developer

            # 3. ignore the first three characters of the third field
            $ sort -k 3.3 data.txt
            01 Joe Sr.Designer
            03 Albert Jr.Designer
            02 Marie Jr.Developer
            04 Dave Sr.Developer

            # 4. sort based on only the third-through-fifth characters of the third field
            $ sort -k 3.3,3.5 data.txt

1. using sort and join together

    1. test data

            $ cat file1.txt
            3       tomato
            1       onion
            4       beet
            2       pepper

            $ cat file2.txt
            4       orange
            3       apple
            1       mango
            2       grapefruit

    1. `sort` these two files and `join` them

            $ join <(sort file1.txt) <(sort file2.txt)
            1 onion mango
            2 pepper grapefruit
            3 tomato apple
            4 beet orange

1. [sort with tab delimiter](http://stackoverflow.com/questions/1037365/unix-sort-with-tab-delimiter)

    1. [ansi-c quoting](http://www.gnu.org/software/bash/manual/bashref.html#ANSI_002dC-Quoting)

            # words of the form `$'string'` are treated specially
            # so `$'\t'` is `horizontal tab`
            $ sort -t$'\t' -k3 -nr data.txt