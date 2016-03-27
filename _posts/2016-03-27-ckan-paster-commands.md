---
layout: post
title: "ckan paster commands"
description: ""
category: [command]
tags: [ckan, paster]
---
{% include JB/setup %}


### commands

			$ paster <ckan commands>

1. enabling ckan commands

			$ paster --plugin=ckan <ckan commands>

	1. pointing to ckan config

			$ paster --plugin=ckan <ckan commands> --config=<config file>

			# e.g.
			$ paster --plugin=ckan db init std.ini

	1. virtual environments

			$ conda create -n ckan2.5 ipython
			$ activate ckan2.5
			[ckan2.5]$ paster --plugin=ckan db init std.ini

	1. running paster on a deployment

			$ sudo -u www-data /var/lib/ckan/std/pyenv/bin/paster --plugin=ckan db init std.ini

	1. getting help on paster

			$ paster --plugin=ckan --help
			$ paster --plugin=ckan --help db

	1. paster executable

			# activating the virtual environments
			$ . pyenv/bin/activate
			# or
			$ pyenv/bin/paster ...

	1. position of paster parameters

			# --plugin is a parameter to paster
			# so needs to come before ckan commands

			# default value for --plugin is setup.py in the current directory
			# you don't need to specify the plugin parameter

			# default value for --config is development.ini

			$ paster --plugin=ckan db --config=development.ini init
			$ paster --plugin=ckan db init --config=development.ini

	1. running a paster shell

			# useful for development
			$ paster --plugin=pylons shell
			$ sudo -u www-data paster --plugin=pylons shell std.ini

1. db manage databases

	1. initialisation

			