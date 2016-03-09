---
layout: post
title: "running scala in sublime text 3 with sbt"
description: ""
category: [lang]
tags: [scala, sbt, st3, SublimeSBT, SublimeREPL]
---
{% include JB/setup %}


### install package with package control

1. install [SublimeREPL](https://packagecontrol.io/packages/SublimeREPL)

1. install [SublimeSBT](https://packagecontrol.io/packages/SublimeSBT)

### usage

1. SublimeREPL

    1. open project

    1. open `scala` source file

    1. `Tools - SublimeREPL - Scala - scala REPL`

1. SublimeSBT

    1. Shift+CMD+P type `sbt: run`

            [info] Set current project to hello (in build file:/Users/hqlgree2/Documents/github/fabric/scala/hello/)
            [info] Updating {file:/Users/hqlgree2/Documents/github/fabric/scala/hello/}root...
            [info] Resolving org.scala-lang#scala-library;2.11.4 ...
            [info] Resolving org.scala-lang#scala-compiler;2.11.4 ...
            [info] Resolving org.scala-lang#scala-reflect;2.11.4 ...
            [info] Resolving org.scala-lang.modules#scala-xml_2.11;1.0.2 ...
            [info] Resolving org.scala-lang.modules#scala-parser-combinators_2.11;1.0.2 ...
            [info] Resolving jline#jline;2.12 ...
            [info] Done updating.
            [info] Compiling 1 Scala source to /Users/hqlgree2/Documents/github/fabric/scala/hello/target/scala-2.11/classes...
            [info] Running Hi 
            Hi!
            [success] Total time: 3 s, completed Aug 1, 2015 10:58:24 PM

             -- Finished --
