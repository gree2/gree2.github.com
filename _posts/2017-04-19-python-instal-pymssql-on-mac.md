---
layout: post
title: "python instal pymssql on mac"
description: ""
category: [python, setup]
tags: [pymssql, mssql, brew, freetds]
---
{% include JB/setup %}


### steps

1. 1st try

    1. install

            $ pip install pymssql
            You can install FreeTDS with Homebrew or MacPorts, or by downloading
              and compiling it yourself.

              Homebrew (http://brew.sh/)
              --------------------------
              brew install freetds

              MacPorts (http://www.macports.org/)
              -----------------------------------
              sudo port install freetds
            ...
            Failed building wheel for pymssql
              Running setup.py clean for pymssql
            ...
            _mssql.c:19425:15: error: use of undeclared identifier 'DBVERSION_80'
                __pyx_r = DBVERSION_80;
                          ^
            1 error generated.
            error: command 'gcc' failed with exit status 1
            ...

1. 2ed try

    1. from [mac-pip-install-pymssql-error](http://stackoverflow.com/questions/37771434/mac-pip-install-pymssql-error)

            $ brew unlink freetds; brew install homebrew/versions/freetds091
            Unlinking /usr/local/Cellar/freetds/1.00.27... 38 symlinks removed
            Updating Homebrew...
            ==> Tapping homebrew/versions
            Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-versions'...
            remote: Counting objects: 9, done.
            remote: Compressing objects: 100% (8/8), done.
            remote: Total 9 (delta 0), reused 6 (delta 0), pack-reused 0
            Unpacking objects: 100% (9/9), done.
            Tapped 0 formulae (37 files, 31.4KB)
            Warning: Use freetds@0.91 instead of deprecated homebrew/versions/freetds091
            ==> Downloading https://homebrew.bintray.com/bottles/freetds@0.91-0.91.112.sierra.bottle.tar.gz
            ######################################################################## 100.0%
            ==> Pouring freetds@0.91-0.91.112.sierra.bottle.tar.gz
            ==> Caveats
            This formula is keg-only, which means it was not symlinked into /usr/local.

            This is an alternate version of another formula.

            If you need to have this software first in your PATH run:
              echo 'export PATH="/usr/local/opt/freetds@0.91/bin:$PATH"' >> ~/.bash_profile

            For compilers to find this software you may need to set:
                LDFLAGS:  -L/usr/local/opt/freetds@0.91/lib
                CPPFLAGS: -I/usr/local/opt/freetds@0.91/include

            ==> Summary
            🍺  /usr/local/Cellar/freetds@0.91/0.91.112: 763 files, 10.2MB

    1. install

            $ pip install pymssql
              _mssql.c:266:10: fatal error: 'sqlfront.h' file not found
              #include "sqlfront.h"
                       ^
              1 error generated.
              error: command 'gcc' failed with exit status 1

              ----------------------------------------
              Failed building wheel for pymssql
              Running setup.py clean for pymssql
            Failed to build pymssql
            Installing collected packages: pymssql
              Running setup.py install for pymssql ... error
            ...
            _mssql.c:435:10: fatal error: 'sqlfront.h' file not found
            #include "sqlfront.h"
                     ^
            1 error generated.
            error: command 'gcc' failed with exit status 1
            ...

1. 3rd try

    1. force link

            $ brew link --force freetds@0.91

    1. install

            $ Collecting pymssql
              Using cached pymssql-2.1.3.tar.gz
            Building wheels for collected packages: pymssql
              Running setup.py bdist_wheel for pymssql ... done
              Stored in directory: /Users/gree2/Library/Caches/pip/wheels/c1/1e/75/bc600eb8a5c9ed77fb1edf15ae5a48b5b427b0390c9a7c9dff
            Successfully built pymssql
            Installing collected packages: pymssql
            Successfully installed pymssql-2.1.3
