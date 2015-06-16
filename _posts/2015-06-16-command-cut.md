---
layout: post
title: "command cut"
description: ""
category: [command]
tags: [cut]
---
{% include JB/setup %}


### info

1. remove or `cut out` sections of each line of a file or files

1. when invoking `cut` use -b -c or -f option, but only one of them

### demo

1. cut field

	1. `second-through-fourth` field of each line

			$ cut -f 2-4 data.txt

	1. `first-through-second` and `fourch-through-fifth` field

			$ cut -f 1-2,4-5 data.txt

	1. third and every field after it

			$ cut -f 3- data.txt

1. specifying a delimiter other than tab

	1. cut by `:`

			$ cut -f 1 -d ':' data.txt

	1. delimited by a space

			$ cut -f 1,3 -d ':' --output-delimiter=' ' data.txt

	1. delimited by `tab`

			$ cut -f 1,3 -d ':' --output-delimiter=$'\t' data.txt

1. character

	1. output third character

			$ cut -c 3 data.txt

	1. output first three characters

			$ cut -c 1,3 data.txt