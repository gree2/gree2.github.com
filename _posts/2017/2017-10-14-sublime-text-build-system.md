---
layout: post
title: "sublime text build system"
description: ""
category: [tools]
tags: [c++, g++, java, python]
---
{% include JB/setup %}


### build system

1. run c++ code

    1. find g++

            $ which g++
            /usr/bin/g++

    1. sublime-build file

            {
                "cmd" : ["/usr/bin/g++ $file_name -std=c++11 -o ${file_base_name} && ./${file_base_name} && rm -rf $file_base_name"],
                "selector" : "source.c, source.c++",
                "shell": true,
                "working_dir" : "$file_path"
            }

1. run java code

    1. sublime-build file

            {
                "shell_cmd": "javac -encoding utf-8 $file_name && java $file_base_name && rm -rf $file_base_name.class",
                "file_regex": "^ *\\[javac\\] (.+):([0-9]+):() (.*)$",
                "selector": "source.java",
                "encoding": "utf-8"
            }

1. run python code in virtual env

    1. activate env

            $ source activate dev
            (dev)$ which python
            /Users/gree2/anaconda2/envs/dev/bin/python

    1. sublime-build file

            {
                "cmd": ["/Users/gree2/anaconda2/envs/dev/bin/python", "-u", "$file"],
                "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
                "selector": "source.python",
                "env": {"PYTHONIOENCODING": "utf8"},
            }
