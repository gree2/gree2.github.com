---
layout: post
title: "getting started with sbt"
description: ""
category: [lang]
tags: [scala, sbt]
---
{% include JB/setup %}


### 1. installing sbt

1. installing sbt on mac

            $ brew install sbt

1. installing sbt on ubuntu

            $ sudo echo "deb http://dl.bintray.com/sbt/debian /" | \
            sudo tee -a /etc/apt/sources.list.d/sbt.list
            $ sudo apt-get update
            $ sudo apt-get install sbt

### 2. hello, world

1. hw.scala

            $ pico hw.scala
            object Hi {
                def main(args: Array[String]) = println("Hi!")
            }

1. build

            $ sbt
            ...
            > run
            ...
            Hi!

1. build definition

            $ pico build.dbt
            lazy val root = (project in file(".")).
              settings(
                name := "hello",
                version = "1.0",
                scalaVersion := "2.11.4"
              )
            # if you plan package your project in a jar
            # you will want to set at least the name and version in a build.sbt

### directory structure

1. `hello` is base directory

1. source code

            src/
                main/
                    resources/
                        <files to include in main jar here>
                    scala/
                        <main scala sources>
                    java/
                        <main java sources>
                test/
                    resources/
                        <files to include in test jar here>
                    scala/
                        <test scala sources>
                    java/
                        <test java sources>

1. sbt build definition files

            build.sbt
            project/
                Build.scala

            # you mac see `.sbt` files inside `project/`
            # but they are not equivalent to `.sbt` in `base directory`

1. build products

            # compiled classes, package jars, managed files, caches, doc
            # will be written to the target directory by default

### 4. running

1. interactive mode

            $ sbt
            > compile
            > run
            > exit
            > Ctrl+D (Unix)
            > Ctrl+Z (Windows)

1. batch mode

            $ sbt clean compile "testOnly TestA TestB"

1. continuous build and test

            > ~ compile

1. common commands

            > clean
            > compile
            > test
            > console
            > run <argument>
            > package
            > help
            > reload

1. tab completion

1. history commands

            > ! 
            > !!
            > !:
            > !:n
            > !n
            > !-n
            > !string
            > !?string

### 5. .sbt build definition

1. three flavors of build definition

    1. multi-project `.sbt`

    1. bare `.sbt`

    1. `.scala`

1. what is a build definition

            # in `build.sbt` create project definition
            lazy val root = (project in file("."))

1. how `build.sbt` defines settings

            lazy val commonSettings = Seq(
                organization := "com.example",
                version := "0.1.0",
                scalaVersion := "2.11.4"
            )

            lazy val root = (project in file(".")).
                settings(commonSettings: _*).
                settings(
                    name := "hello"
                )

1. keys

    1. types

            SettingKey[T]
            TaskKey[T]
            InputKey[T]

    1. build-in keys

            # a `build.sbt` implicitly has an
            import sbt.Keys._
            # so `sbt.Keys.name can be referred to as `name`

    1. custom keys

            # define a key for a new task called `hello`
            lazy val hello = taskKey[Unit]("An example task")

    1. task vs setting keys

            # a TaskKey[T] is said to define a `task`
            # tasks are operations such as `compile` or `package`
            # they may return `Unit` (Unit is scala for void)

            # `package` is a `TaskKey[File]`
            # and its value is the `jar` file it creates

1. defining tasks and settings

    1. use `:=` you can assign a value to a setting and a computation to a task

    1. types for tasks and settings

1. keys in sbt interactive mode

1. imports in build.sbt

            import sbt._
            import Process._
            import Kyes._

1. adding library dependencies

            val derby = "org.apache.derby" % "derbt" % "10.4.1.3"

            lazy val commonSettings = Seq(
                organization := "com.example",
                version := "0.1.0",
                scalaVersion := "2.11.4"
            )

            lazy val root = (project in file(".")).
                settings(commonSettings: _*).
                settings(
                    name := "hello",
                    libraryDependencies += derby
                )

### 6. scopes