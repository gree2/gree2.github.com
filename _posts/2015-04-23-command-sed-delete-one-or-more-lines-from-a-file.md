---
layout: post
title: "command sed delete one or more lines from a file"
description: ""
category: [command]
tags: [sed, delete, line]
---
{% include JB/setup %}


### syntax

[reference](http://en.kioskea.net/faq/1451-sed-delete-one-or-more-lines-from-a-file)

			sed '{[/]<n>|<string>|<regex>[/]}d' <fileName> 
			sed '{[/]<adr1>[,<adr2>][/]d' <fileName>

			/.../=delimiters
			n = line number
			string = string found in in line
			regex = regular expression corresponding to the searched pattern
			addr = address of a line (number or pattern )
			d = delete

### examples

1. remove 3rd line

			$ sed '3d' filename.txt

1. remove the line containning `awk`

			$ sed '/awk/d' filename.txt

1. remove the last line

			$ sed '$d' filename.txt

1. remove all empty lines

			$ sed '/^$/d' filename.txt
			$ sed '/./!d' filename.txt

1. remove the line matching by a regular expresstion

			$ sed '/[0-9/][0-9]*$/d' filename.txt

1. remove lines between 7 and 9

			$ sed '7,9d' filename.txt

			$ sed '/-start/,/-end/d' filename.txt

* all above examples are only changed at the display of the file

* permanent change

			$ sed -i '2d' filename.txt
			# delete 2ed line and save to filename.txt
			
			$ sed -i".bak" '2d' filename.txt
			# backup filename.txt to filename.txt.bak
			# delete 2ed line and save to filename.txt