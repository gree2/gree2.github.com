---
layout: post
title: "android studio could not reserve enough space for object heap"
description: ""
category: [android]
tags: [heap, vm]
---
{% include JB/setup %}

### problem

    Failed to import new Gradle project: Could not fetch model of type 'IdeaProject' using Gradle distribution 'http://services.gradle.org/distributions/gradle-1.6-bin.zip'.
    Unable to start the daemon process.
    This problem might be caused by incorrect configuration of the daemon.
    For example, an unrecognized jvm option is used.
    Please refer to the user guide chapter on the daemon at http://gradle.org/docs/1.6/userguide/gradle_daemon.html
    Please read below process output to find out more:
    -----------------------
    Error occurred during initialization of VM
    Could not reserve enough space for object heap
    Error: Could not create the Java Virtual Machine.
    Error: A fatal exception has occurred. Program will exit.


    Consult IDE log for more details (Help | Show Log)

### solution

1. `Ctrl+Alt+S` open `Settings`

1. search `Compiler`

1. `VM Options` fill `-Xmx512m`

### references

* [stackoverflow](http://stackoverflow.com/questions/16712413/an-error-when-creating-new-project-in-android-studio)