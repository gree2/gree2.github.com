---
layout: post
title: "play with r on mac"
description: ""
category: [bigdata]
tags: [brew, r, tap]
---
{% include JB/setup %}


### install

1. check this [post](http://stackoverflow.com/questions/20457290/installing-r-with-homebrew)

    1. brew tap

            $ brew tap homebrew/science
            ==> Tapping homebrew/science
            Cloning into '/usr/local/Library/Taps/homebrew/homebrew-science'...
            remote: Counting objects: 572, done.
            remote: Compressing objects: 100% (571/571), done.
            remote: Total 572 (delta 1), reused 57 (delta 0), pack-reused 0
            Receiving objects: 100% (572/572), 464.48 KiB | 128.00 KiB/s, done.
            Resolving deltas: 100% (1/1), done.
            Checking connectivity... done.
            Tapped 566 formulae (593 files, 3.0M)

    1. brew install

            $ brew install r
            ==> Installing r from homebrew/homebrew-science
            ==> Installing dependencies for homebrew/science/r: gmp, mpfr, libmpc, is
            ==> Installing homebrew/science/r dependency: gmp
            ==> Downloading https://homebrew.bintray.com/bottles/gmp-6.1.0.el_capitan.bottle
            ######################################################################## 100.0%
            ==> Pouring gmp-6.1.0.el_capitan.bottle.tar.gz
            🍺  /usr/local/Cellar/gmp/6.1.0: 15 files, 3.2M
            ==> Installing homebrew/science/r dependency: mpfr
            ==> Downloading https://homebrew.bintray.com/bottles/mpfr-3.1.3.el_capitan.bottl
            ######################################################################## 100.0%
            ==> Pouring mpfr-3.1.3.el_capitan.bottle.tar.gz
            🍺  /usr/local/Cellar/mpfr/3.1.3: 24 files, 3.6M
            ==> Installing homebrew/science/r dependency: libmpc
            ==> Downloading https://homebrew.bintray.com/bottles/libmpc-1.0.3.el_capitan.bot
            ######################################################################## 100.0%
            ==> Pouring libmpc-1.0.3.el_capitan.bottle.tar.gz
            🍺  /usr/local/Cellar/libmpc/1.0.3: 10 files, 380K
            ==> Installing homebrew/science/r dependency: isl
            ==> Downloading https://homebrew.bintray.com/bottles/isl-0.15.el_capitan.bottle.
            ######################################################################## 100.0%
            ==> Pouring isl-0.15.el_capitan.bottle.tar.gz
            🍺  /usr/local/Cellar/isl/0.15: 69 files, 3.8M
            ==> Installing homebrew/science/r dependency: gcc

    1. brew install again

            $ brew install r
            ==> Installing r from homebrew/homebrew-science
            ==> Installing dependencies for homebrew/science/r: gcc, jpeg, libtiff, p
            ==> Installing homebrew/science/r dependency: gcc
            ==> Downloading http://ftpmirror.gnu.org/gcc/gcc-5.3.0/gcc-5.3.0.tar.bz2
            ==> Downloading from http://mirrors.ustc.edu.cn/gnu/gcc/gcc-5.3.0/gcc-5.3.0.tar.
            ######################################################################## 100.0%
            ==> Patching
            patching file gcc/jit/Make-lang.in
            patching file gcc/jit/jit-playback.c
            Hunk #1 succeeded at 2470 with fuzz 2 (offset 54 lines).
            ==> ../configure --build=x86_64-apple-darwin15.2.0 --prefix=/usr/local/Cellar/gc
            ==> make bootstrap
            ==> make install
            ==> Caveats
            GCC has been built with multilib support. Notably, OpenMP may not work:
              https://gcc.gnu.org/bugzilla/show_bug.cgi?id=60670
            If you need OpenMP support you may want to
              brew reinstall gcc --without-multilib
            ==> Summary
            🍺  /usr/local/Cellar/gcc/5.3.0: 1361 files, 258M, built in 33.0 minutes
            ==> Installing homebrew/science/r dependency: jpeg
            ==> Downloading https://homebrew.bintray.com/bottles/jpeg-8d.el_capitan.bottle.2
            ######################################################################## 100.0%
            ==> Pouring jpeg-8d.el_capitan.bottle.2.tar.gz
            🍺  /usr/local/Cellar/jpeg/8d: 18 files, 760K
            ==> Installing homebrew/science/r dependency: libtiff
            ==> Downloading https://homebrew.bintray.com/bottles/libtiff-4.0.6.el_capitan.bo
            ######################################################################## 100.0%
            ==> Pouring libtiff-4.0.6.el_capitan.bottle.tar.gz
            🍺  /usr/local/Cellar/libtiff/4.0.6: 259 files, 3.9M
            ==> Installing homebrew/science/r dependency: pcre
            ==> Downloading https://homebrew.bintray.com/bottles/pcre-8.38.el_capitan.bottle
            ######################################################################## 100.0%
            ==> Pouring pcre-8.38.el_capitan.bottle.tar.gz
            🍺  /usr/local/Cellar/pcre/8.38: 146 files, 5.9M
            ==> Installing homebrew/science/r dependency: libpng
            ==> Downloading https://homebrew.bintray.com/bottles/libpng-1.6.19.el_capitan.bo
            ######################################################################## 100.0%
            ==> Pouring libpng-1.6.19.el_capitan.bottle.tar.gz
            🍺  /usr/local/Cellar/libpng/1.6.19: 17 files, 1.2M
            ==> Installing homebrew/science/r dependency: xz
            ==> Downloading https://homebrew.bintray.com/bottles/xz-5.2.2.el_capitan.bottle.
            ######################################################################## 100.0%
            ==> Pouring xz-5.2.2.el_capitan.bottle.tar.gz
            🍺  /usr/local/Cellar/xz/5.2.2: 59 files, 1.7M
            ==> Installing homebrew/science/r dependency: freetype
            ==> Downloading https://homebrew.bintray.com/bottles/freetype-2.6_1.el_capitan.b
            ######################################################################## 100.0%
            ==> Pouring freetype-2.6_1.el_capitan.bottle.tar.gz
            🍺  /usr/local/Cellar/freetype/2.6_1: 60 files, 2.6M
            ==> Installing homebrew/science/r dependency: fontconfig
            ==> Downloading https://homebrew.bintray.com/bottles/fontconfig-2.11.1.el_capita
            ######################################################################## 100.0%
            ==> Pouring fontconfig-2.11.1.el_capitan.bottle.4.tar.gz
            ==> /usr/local/Cellar/fontconfig/2.11.1/bin/fc-cache -frv
            🍺  /usr/local/Cellar/fontconfig/2.11.1: 449 files, 3.9M
            ==> Installing homebrew/science/r dependency: pixman
            ==> Downloading https://homebrew.bintray.com/bottles/pixman-0.32.8.el_capitan.bo
            ######################################################################## 100.0%
            ==> Pouring pixman-0.32.8.el_capitan.bottle.tar.gz
            🍺  /usr/local/Cellar/pixman/0.32.8: 11 files, 1.2M
            ==> Installing homebrew/science/r dependency: cairo
            ==> Downloading https://homebrew.bintray.com/bottles/cairo-1.14.4.el_capitan.bot
            ######################################################################## 100.0%
            ==> Pouring cairo-1.14.4.el_capitan.bottle.tar.gz
            🍺  /usr/local/Cellar/cairo/1.14.4: 112 files, 6.1M
            ==> Installing homebrew/science/r
            ==> Downloading https://homebrew.bintray.com/bottles-science/r-3.2.3.el_capitan.
            ######################################################################## 100.0%
            ==> Pouring r-3.2.3.el_capitan.bottle.tar.gz
            During startup - Warning messages:
            1: Setting LC_COLLATE failed, using "C" 
            2: Setting LC_TIME failed, using "C" 
            3: Setting LC_MESSAGES failed, using "C" 
            4: Setting LC_MONETARY failed, using "C" 
            During startup - Warning messages:
            1: Setting LC_COLLATE failed, using "C" 
            2: Setting LC_TIME failed, using "C" 
            3: Setting LC_MESSAGES failed, using "C" 
            4: Setting LC_MONETARY failed, using "C" 
            ==> Caveats
            To enable rJava support, run the following command:
              R CMD javareconf JAVA_CPPFLAGS=-I/System/Library/Frameworks/JavaVM.framework/Headers
            If you've installed a version of Java other than the default, you might need to instead use:
              R CMD javareconf JAVA_CPPFLAGS="-I/System/Library/Frameworks/JavaVM.framework/Headers -I/Library/Java/JavaVirtualMachines/jdk<version>.jdk/"
            (where <version> can be found by running `java -version`, `/usr/libexec/java_home`, or `locate jni.h`), or:
              R CMD javareconf JAVA_CPPFLAGS="-I/System/Library/Frameworks/JavaVM.framework/Headers -I$(/usr/libexec/java_home | grep -o '.*jdk')"

            Bash completion has been installed to:
              /usr/local/etc/bash_completion.d
            ==> Summary
            🍺  /usr/local/Cellar/r/3.2.3: 2176 files, 59M

1. install SublimeREPL

    1. install it with package control

    1. `Tools` -> `SublimeREPL` -> `R`

            R version 3.2.3 (2015-12-10) -- "Wooden Christmas-Tree"
            Copyright (C) 2015 The R Foundation for Statistical Computing
            Platform: x86_64-apple-darwin15.2.0 (64-bit)

            R is free software and comes with ABSOLUTELY NO WARRANTY.
            You are welcome to redistribute it under certain conditions.
            Type 'license()' or 'licence()' for distribution details.

              Natural language support but running in an English locale

            R is a collaborative project with many contributors.
            Type 'contributors()' for more information and
            'citation()' on how to cite R or R packages in publications.

            Type 'demo()' for some demos, 'help()' for on-line help, or
            'help.start()' for an HTML browser interface to help.
            Type 'q()' to quit R.

            During startup - Warning messages:
            1: Setting LC_CTYPE failed, using "C" 
            2: Setting LC_COLLATE failed, using "C" 
            3: Setting LC_TIME failed, using "C" 
            4: Setting LC_MESSAGES failed, using "C" 
            5: Setting LC_MONETARY failed, using "C" 
            > 
