---
layout: post
title: "command tee"
description: ""
category: [command]
tags: [tee]
---
{% include JB/setup %}


### tee

1. info

	1. `redirect` data to a file

	1. and provide a `copy` of redirect data as `stdin` for next set of commands

	1. read from `stdin` only

1. demo

	1. create 2 files

			$ echo a1 > a1.txt
			$ echo a2 > a2.txt

	1. write a*.txt to out.txt and print out

			$ cat a* | tee out.txt | cat -n
				1 a1
				2 a2

			$ cat out.txt
			a1
			a2

	1. default tee overwrite the file

			# using `-a` to append
			$ cat a* | tee -a out.txt | cat -n

	1. use `stdin` as a command argument using `-`

			$ echo who is this | tee -
