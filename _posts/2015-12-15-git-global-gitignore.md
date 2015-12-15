---
layout: post
title: "git global gitignore"
description: ""
category: [git]
tags: [git, config, gitignore]
---
{% include JB/setup %}


### steps

1. edit `.gitconfig` file

	1. add `excludesfile` line to `core` section

			[core]
				autocrlf = true
				excludesfile = c:/Users/Administrator/.gitignore_global

	1. save this file

1. create a new file `.gitignore_blobal`

	1. add file or folder to ignore

			# eg.
			.idea/
			*.iml

1. done