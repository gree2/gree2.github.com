---
layout: post
title: "command cat"
description: ""
category: [command]
tags: [cat]
---
{% include JB/setup %}


### info

1. `cat` stands for `catenate`

	1. read data from files

	1. output file contents

1. can be used to

	1. display text fies

	1. copy text files into new file

	1. append contents of a text file to the end of another text file

### usage

1. displaying text files

			$ cat textfile

			$ cat textfile1 textfile2

1. copy a text file

	1. shell redirection

			$ cat textfile1 > textfile2

	1. catenate serveral files into destination file

			$ cat textfile1 textfile2 > textfile3

1. append text file to another text file

			$ cat textfile1 >> textfile2

			$ cat textfile1 textfile2 >> textfile3

### options

1. `-A` equivalent to `-vET`

1. `-E` display "$" at end of each line

1. `-n` number all output lines

1. `-T` display `TAB` characters as `^I`