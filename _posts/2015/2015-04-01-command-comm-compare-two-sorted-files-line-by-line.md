---
layout: post
title: "command comm compare two sorted files line by line"
description: ""
category: [command]
tags: [linux, command, comm]
---
{% include JB/setup %}

### about comm

compare two sorted files line-by-line

### syntax

comm [option]... file1 file2

### description

compare sorted files `file1` and `file2`

with no options, `comm` produces three-column output

* column 1 contains lines unique to `file1`
* column 2 contains lines unique to `file2`
* column 3 contains lines common to both files

each of these columns can be suppressed individually with options

### options

* `-1` suppress column 1 (lines unique to file1)
* `-2` suppress column 2 (lines unique to file2)
* `-3` suppress column 3 (lines that appear in both files)
* `--check-order` chech that the input is correctly sorted, even if all input lines are pairable
* `--nocheck-order` do not check that the input is correctly sorted
* `--output-delimiter=str` separate columns with string `str`
* `--help` display a help message and exit
* `--version` output version infomation and exit

### examples

1. print only the lines present in `both` file1.txt and file2.txt

		comm -12 file1.txt file2.txt

1. print only the lines that are present in file1 and not in file2 and vice versa

		comm -3 file1.txt file2.txt

### references

[computerhope](http://www.computerhope.com/unix/ucomm.htm)