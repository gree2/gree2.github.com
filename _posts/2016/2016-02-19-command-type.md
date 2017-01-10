---
layout: post
title: "command type"
description: ""
category: [command]
tags: [type, file, which, paste, bc]
---
{% include JB/setup %}


### info

1. display information about command type

	1. an executable program

			like all those tiles we saw in /urs/bin

			programs can be
			1. compiled binaries such as programs written in c or c++
			2. programs written in scripting languages such as the shell, perl, python, ruby

	1. a command built into the shell itself

			bash provides a number of commands internally called shell builtins

			the `cd` command is a shell builtin

	1. a shell function

			these are miniature shell scripts incorporated into the env

			$ fac() { (echo 1; seq $1) | paste -s -d\* | bc; }
			$ fac 5
			120

	1. an alias

			commands that you can define yourselves
			built from other commands

			$ alias l='ls -1 --group-directories-first'

### usage

1. builtin

			$ type -a pwd
			pwd is a shell builtin
			pwd is /bin/pwd

1. function

			$ type -a fac
			fac is a function
			fac ()
			{
				( echo 1;
				seq $1 ) | paste -s -d\* | bc
			}

1. alias

			$ type -a l
			l is aliased to `ls -1 --group-directories-first`
