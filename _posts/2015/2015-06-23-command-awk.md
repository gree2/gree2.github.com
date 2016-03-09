---
layout: post
title: "command awk"
description: ""
category: [command]
tags: [awk, text processing]
---
{% include JB/setup %}


### info

1. short for `Aho, Weinberger, and Kernighan`

1. `awk` is an interpreted programming language which focuses on processing text

1. `awk` is a direct predecessor of `perl`

### variable

1. `CONVFMT` conversion format used when converting numbers (default %.6g)

1. `FS` regular expression used to separate fields; settable by option -Ffs

1. `NF` number of fields in the current record

1. `NR` ordinal number of the current record

1. `FNR` ordinal number of the current record in the current file

1. `FILENAME` the name of the current input file

1. `RS` input record separator (default newline)

1. `OFS` output field separator (default blank)

1. `ORS` output record separator (default newline)

1. `OFMT` output format for numbers

1. `SUBSEP` character to separate multiple subscripts (default 034 double quotes)

1. `argc` argument count, assignable

1. `argv` argument array, assignable; non-null members are taken as filenames

1. `ENVIRON` array of environment variables; subscripts are names

### build in functions

1. mathenmatical

            exp, log, sqrt, sin, cos, atan2

1. other

            length rand srand int substr index match
            split sub gsub sprintf, system tolower toupper

### demo

1. [computerhope](http://www.computerhope.com/unix/uawk.htm)

    1. print only lines longer than 72 characters

            $ awk 'length($0) > 72' text.txt

    1. print 1st two fields of data in oppeosite order

            $ pico text.txt
            red apple blue ...

            $ awk '{ print $2, $1 }' text.txt
            apple red blue ...

    1. execute awk program in `prog.awk`

            $ awk -f prog.awk text.txt

            # add up first column of input file
            # print `sum` and `average`
            $ pico prog.awk
            { s += $1 }
            END { print "sum is", s, " average is", s/NR }

            # print all lines found between `start` and `stop`
            $ pico prog.awk
            /start/, /stop/

            # simulates `echo` command
            $ pico prog.awk
            BEGIN {
            for (i = 1; i < ARGC; i++) pirntf "%s ", ARGV[i]
            printf "\n"
            exit }

1. [thegeekstuff demo](http://www.thegeekstuff.com/2010/11/50-linux-commands/)

    1. remove duplicate lines using awk

            $ awk '!($0 in array) { array[$0]; print }' temp

    1. print all lines from `/etc/passwd` that has the same `uid` and `gid`

            $ awk -F ':' '$3==$4' /etc/passwd

    1. print only specific field from a file

            $ awk '{print $2,$5;}' text.txt

1. [thegeekstuff demo powerful build-in variables](http://www.thegeekstuff.com/2010/01/8-powerful-awk-built-in-variables-fs-ofs-rs-ors-nr-nf-filename-fnr/)

    1. `FS` input field separator

            # 1. using `-F` command line option
            $ awk -f 'FS' 'commands' input.txt

            # 2. can be set like normal variable
            $ awk 'BEGIN{FS="FS";}'

            $ pico prog.awk
            BEGIN{
            FS=":";
            print "name\tuserid\tgroupid\thomedirectory";
            }
            {
                print $1"\t"$3"\t"$4"\t"$6;
            }
            END {
                print NR, "records processed";
            }

            $ awk -f prog.awk /etc/passwd

    1. `OFS` output field separator

            # default is a single space character
            $ awk -F':' '{print $3,$4;}' /etc/passwd

            $ awk -F':' 'BEGIN{OFS="=";} {print $3,$4;}' /etc/passwd

    1. `RS` record separator

            # RS defines a line
            # awk reads line by line by default

            # separated by double new line
            $ pico student.txt
            Jones
            2143
            78
            84
            77


            Gondrol
            2321
            56
            58
            45


            RinRao
            2122
            38
            37
            65

            $ pico prog.awk
            BEGIN {
                RS="\n\n";
                FS="\n";
            }
            {
                print $1,$2;
            }

            $ awk -f prog.awk student.txt
            Jones 2143
            Gondrol 2321
            RinRao 2122

    1. `ORS` output record separator

            $ awk 'BEGIN{ORS="=";} {print;}' student.txt
            Jones 2143 78 84 77=Gondrol 2321 56 58 45=RinRao 2122 38 37 65

    1. `NR` number of records

            # total number of records being processed
            # or line number

            # in the `END` section
            # `NR` tells total number of records in a file
            $ awk '{print "processing record - ",NR;}END {print NR, "student records are processed"}'
            Processing Record -  1
            Processing Record -  2
            Processing Record -  3
            3 Students Records are processed

    1. `NF` number of fields in a record

            # total number of fields in a record
            # validating whether all the fields are exist in a record

            $ pico student.txt
            Jones 2143 78 84 77
            Gondrol 2321 56 58 45
            RinRao 2122 38 37

            $ awk '{print NR,"->",NF}' student.txt
            1 -> 5
            2 -> 5
            3 -> 4

    1. `FILENAME` name of the current input file

            $ awk '{print FILENAME}' student.txt
            student.txt
            student.txt
            student.txt

    1. `FNR` number of records relative to the current input file

            # reads from multiple input file
            # `NR` variable will give total number of records
            # relative to all the input file

            # `FNR` will give number of records for each input file

            $ awk 'print FILENAME, FNR;' student.txt book.txt
            student.txt 1
            student.txt 2
            student.txt 3
            book.txt 1
            book.txt 2
            book.txt 3