---
layout: post
title: "git tools submodules"
description: ""
category: [git]
tags: [submodule]
---
{% include JB/setup %}

### starting with submodules

	$ git submodule add https://github.com/Alamofire/Alamofire.git

by default, submodules will add the subproject into a dir named the same as the repo
in this case `Alamofire`

### cloning project with submodules

	$ git clone https://github.com/gree2/EasternPioneerBusSwift

you must run 2 commands

	$ git submodule init
	# to initialize your local configuration file

	$ git submodule update
	# to fetch all the data from that project

	$ git clone --recursive https://github.com/gree2/EasternPioneerBusSwift
	# it will automatically initialize and update each submodule

	Cloning into 'EasternPioneerBusSwift'...
	remote: Counting objects: 265, done.
	remote: Compressing objects: 100% (27/27), done.
	remote: Total 265 (delta 10), reused 0 (delta 0), pack-reused 236
	Receiving objects: 100% (265/265), 15.33 MiB | 357.00 KiB/s, done.
	Resolving deltas: 100% (93/93), done.
	Checking connectivity... done.
	Submodule 'Alamofire' (http://github.com/Alamofire/Alamofire.git) registered for path 'Alamofire'
	Cloning into 'Alamofire'...
	remote: Counting objects: 976, done.
	remote: Total 976 (delta 0), reused 0 (delta 0), pack-reused 976
	Receiving objects: 100% (976/976), 494.99 KiB | 24.00 KiB/s, done.
	Resolving deltas: 100% (522/522), done.
	Checking connectivity... done.
	Submodule path 'Alamofire': checked out '90c4d77be8af79fba58f276bc8f6bbc30e35c8d3'

### working on project with submodule

want to check for new work in a submodule

go into the dir

	$ git fetch
	$ git merge origin/master

	if you go back into the main project
	$ git diff --submodule
	you can see that the submodule was updated and
	get a list of commits that were added to it

	$ git submodule update --remote Alamofire
	an easier way to do this
	git will to into your submodules and fetch  and update for you

this command will by default assume that

you want to update the checkout to the `master` branch of the submodule repo

if you want to have the submodule track that repo's `stable` branch

	$ git config -f .gitmodules submodule.Alamofire.branch stable
	$ git submodule update --remote

### working on submodule

go into our submodule dir and check out a branch

	$ git checkout stable
	$ git submodule update --remote --merge
	there were some changes on the server for this submodule and it gets merged in

	$ cd Alamofire
	$ pico some_code.swift
	$ git commit -am "some messages"

if we update our submodule we can see what happens

when we have mode a local change and upstream also has a change we need to incorporate