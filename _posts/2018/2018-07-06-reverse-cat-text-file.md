---
layout: post
title: "reverse cat text file"
description: ""
category: [command]
tags: [cat, tac, sed]
---
{% include JB/setup %}


### methods

1. tac

    1. install on mac

            $ brew install coreutils
            ==> Downloading https://ftp.gnu.org/gnu/coreutils/coreutils-8.30.tar.xz
            ######################################################################## 100.0%
            ==> ./configure --prefix=/usr/local/Cellar/coreutils/8.30 --program-prefix=g --without-gmp
            ==> make install
            ==> Caveats
            All commands have been installed with the prefix 'g'.

            If you really need to use these commands with their normal names, you
            can add a "gnubin" directory to your PATH from your bashrc like:

                PATH="/usr/local/opt/coreutils/libexec/gnubin:$PATH"

            Additionally, you can access their man pages with normal names if you add
            the "gnuman" directory to your MANPATH from your bashrc as well:

                MANPATH="/usr/local/opt/coreutils/libexec/gnuman:$MANPATH"

            ==> Summary
            🍺  /usr/local/Cellar/coreutils/8.30: 430 files, 8.8MB, built in 4 minutes 26 seconds

    1. [create a link](https://unix.stackexchange.com/questions/114041/how-can-i-get-the-tac-command-on-os-x)

            $ ln -s /usr/local/bin/gtac /usr/local/bin/tac

1. cat

    1. use -r

            $ cat -r file.txt

1. sed

    1. use cat -n

            $ cat -n file.txt | grep needle

            11344 sth bla bla needle bla bla sth

    1. use sed -n

            $ sed -n 11344,11444p file.txt
            ...
