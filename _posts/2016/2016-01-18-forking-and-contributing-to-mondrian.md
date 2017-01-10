---
layout: post
title: "forking and contributing to mondrian"
description: ""
category: [bi]
tags: [git, fork, branch, checkout]
---
{% include JB/setup %}


### fork

1. fork from [mondrian](http://github.com/pentaho/mondrian) use your github account

1. check this [wiki](https://github.com/pentaho/mondrian/wiki/Forking-and-Contributing-to-Mondrian) for details

### cloning a specific branch

1. clone the repo

			$ git clone https://github.com/gree2/mondrian.git

1. list avaiable branches

			$ git branch -a

1. checkout the branch you want to work

			$ git checkout 4.4

### adding upstream and local settings

1. keep your repo uptodate with pentaho mondrian repo

			$ git remote add upstream git://github.com/pentaho/mondrian.git

1. disable fast-forward during merges

			# to preserve the entire commit history
			# and properly track file changes
			$ git config --add merge.ff false

### merging upstream changes

1. to update and merge changes from the upstream repo

			# where [branch] is the current branch you're working on
			$ git fetch upstream [branch]
			$ git fetch upstream/[branch]

1. if no merge conflict, your local repo is now uptodate

### verifying the coding style

1. run following command

			$ ./bin/checkFile.sh --under [e.g. "./src"]
