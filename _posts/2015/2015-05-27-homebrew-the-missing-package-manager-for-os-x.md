---
layout: post
title: "homebrew the missing package manager for os x"
description: ""
category: [tool]
tags: [homebrew, brew]
---
{% include JB/setup %}


### first thing first

            $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

### usage

1. [from http://brew.sh/](http://brew.sh/)

    1. homebrew install `the stuff you need` that appble didn't

            $ brew install wget

    1. homebrew installs packages to their own directory and `symlinks` into `/usr/local`

    1. homebrew won't install files outside its prefix

    1. it's all `git` and `ruby` underneath

    1. homebrew `formula` are simple `ruby scripts`

1. [from tips n' tricks](https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/Tips-N'-Tricks.md)

    1. quickly remove something from /usr/local

            $ brew unlink formula

    1. command tab-completion

            # add to ~/.bash_profile
            source $(brew --repository)/Library/Contributions/brew_bash_completion.sh

    1. pre-downloading a file for a formula

            # download the file and drop it in
            # `/Library/Caches/Homebrew`
            # find cache folder
            $ brew --cache

            # homebrew downloads files as
            # `formula-version`

            # eg. erlang
            # `otp_src_R13B03` to `erlang-R13B03`

            $ mv the_tarball `brew --cache formula-name`

            # pre-cache the download
            $ brew fetch formula
            # also displays `sha1` and `sha256`

    1. using homebrew behind a proxy

            # homebrew uses commands for downloading files
            # curl git svn
            # curl supports
            #      http_proxy
            #      HTTPS_PROXY
            #      FTP_PROXY
            #      GOPHER_PROXY
            #      ALL_PROXY
            #      NO_PROXY

            # `http_proxy` is enough
            $ http_proxy=http://<host>:<port> brew install formula

            # proxy authentication
            $ http_proxy=http://<username>:<password>@<host>:<port> brew install formula

            # this technique will also work with `sudo`
            # but `sudo` clears the environment
            # before executing homebrew
            # your proxy setting may get lost
            $ http_proxy=http://<host>:<port> sudo -E brew install formula


    1. install stuff without xcode-clt

            $ brew sh
            $ gem install ronn

            # this imports brew env into your existing shell
            # gem will pick up the env variables
            # and be able to build

    1. install only a formula's dependencies

            $ brew install --only-dependencies formula

    1. brew irb

            $ brew irb
            ==> Interactive Homebrew Shell
            Example commands available with: brew irb --examples
            irb(main):001:0> 

    1. hiding the beer mug emoji when finishing a build

            $ export HOMEBREW_NO_EMOJI=1

    1. [dependencies](http://zanshin.net/2014/02/03/how-to-list-brew-dependencies/)

            $ brew deps formula

            $ brew uses --installed formula

1. [search](http://superuser.com/questions/390191/where-can-i-find-a-list-of-all-formulas-available-for-homebrew)

    1. list all installed formula

            $ brew list

    1. list all formula

            $ brew search

    1. search a formula

            $ brew search flume
            No formula found for "flume".
            Searching pull requests...
            Closed pull requests:
            Bug fix for flume Formula (https://github.com/Homebrew/homebrew/pull/9689)
            Added a formula for cloudera's flume (https://github.com/Homebrew/homebrew/pull/5271)
            Upgrading flume to the newest version 0.9.4-cdh3u2 (https://github.com/Homebrew/homebrew/pull/8333)
            Add a formula for Apache Flume NG ("next gen"). (https://github.com/Homebrew/homebrew/pull/15249)

1. upgrade

    * list outdated

            # list outdated
            $ brew outdated
            apache-spark (1.3.0 < 1.3.1_1)
            cmake (3.2.1 < 3.2.2)
            glib (2.42.2 < 2.44.1)
            libmagic (5.22 < 5.22_1)
            mysql (5.6.23 < 5.6.24)

    * upgrade glib

            # upgrade
            $ brew upgrade glib
            ==> Upgrading 1 outdated package, with result:
            glib 2.44.1
            ==> Upgrading glib
            ==> Downloading https://homebrew.bintray.com/bottles/glib-2.44.1.yosemite.bottle
            ######################################################################## 100.0%
            ==> Pouring glib-2.44.1.yosemite.bottle.tar.gz
            🍺  /usr/local/Cellar/glib/2.44.1: 416 files, 18M

    * upgrade cmake and libmagic

            $ brew upgrade cmake libmagic
            ==> Upgrading 2 outdated packages, with result:
            cmake 3.2.2, libmagic 5.22_1
            ==> Upgrading cmake
            ==> Downloading https://homebrew.bintray.com/bottles/cmake-3.2.2.yosemite.bottle.3.tar.gz
            ######################################################################## 100.0%
            ==> Pouring cmake-3.2.2.yosemite.bottle.3.tar.gz
            ==> Caveats
            Bash completion has been installed to:
              /usr/local/etc/bash_completion.d
            ==> Summary
            🍺  /usr/local/Cellar/cmake/3.2.2: 1850 files, 32M
            ==> Upgrading libmagic
            ==> Downloading https://homebrew.bintray.com/bottles/libmagic-5.22_1.yosemite.bottle.tar.gz
            ######################################################################## 100.0%
            ==> Pouring libmagic-5.22_1.yosemite.bottle.tar.gz
            🍺  /usr/local/Cellar/libmagic/5.22_1: 272 files, 4.7M
            hqlgree2:~ hqlgree2$ brew outdated

    * update brew

            # update brew
            $ brew update

    * upgrade all formula

            # upgrade all formula
            $ brew upgrade
            Warning: brew upgrade with no arguments will change behaviour soon!
            It currently upgrades all formula but this will soon change to require '--all'.
            Please update any workflows, documentation and scripts!
            ==> Upgrading 3 outdated packages, with result:
            hive 1.1.1, libav 11.4, mysql 5.6.25
            ==> Upgrading hive
            ==> Downloading https://www.apache.org/dyn/closer.cgi?path=hive/hive-1.1.1/apache-hive-1.1.1-bin.tar.
            ^C
            hqlgree2:conf hqlgree2$ brew upgrade libav
            ==> Upgrading 1 outdated package, with result:
            libav 11.4
            ==> Upgrading libav
            ==> Downloading https://homebrew.bintray.com/bottles/libav-11.4.yosemite.bottle.tar.gz
            ######################################################################## 100.0%
            ==> Pouring libav-11.4.yosemite.bottle.tar.gz
            🍺  /usr/local/Cellar/libav/11.4: 8 files, 18M
            hqlgree2:conf hqlgree2$ brew outdated
            hive (1.1.0 < 1.1.1)
            mysql (5.6.23 < 5.6.25)
            hqlgree2:conf hqlgree2$ brew upgrade mysql
            ==> Upgrading 1 outdated package, with result:
            mysql 5.6.25
            ==> Upgrading mysql
            ==> Downloading https://homebrew.bintray.com/bottles/mysql-5.6.25.yosemite.bottle.tar.gz
            ######################################################################## 100.0%
            ==> Pouring mysql-5.6.25.yosemite.bottle.tar.gz
            ==> Caveats
            A "/etc/my.cnf" from another install may interfere with a Homebrew-built
            server starting up correctly.

            To connect:
                mysql -uroot

            To have launchd start mysql at login:
                ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
            Then to load mysql now:
                launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
            Or, if you don't want/need launchctl, you can just run:
                mysql.server start
            ==> Summary
            🍺  /usr/local/Cellar/mysql/5.6.25: 9833 files, 339M

1. uninstall old versions of a formula

    1. remove them simple use

            $ brew cleanup formula

    1. clean up everything at once

            $ brew cleanup

    1. to see what would be cleaned up

            $ brew cleanup -n

### hadoop ecosystem installation

1. [2015-04-16-apache-spark]({% post_url 2015-04-16-apache-spark %})

1. [2015-04-17-apache-hadoop-on-mac-osx-yosemite]({% post_url 2015-04-17-apache-hadoop-on-mac-osx-yosemite %})

1. [2015-04-17-apache-spark-with-python-notebook-on-mac]({% post_url 2015-04-17-apache-spark-with-python-notebook-on-mac %})

1. [2015-05-19-apache-oozie-workflow-scheduler-for-hadoop]({% post_url 2015-05-19-apache-oozie-workflow-scheduler-for-hadoop %})

1. [2015-05-19-impala-native-analytic-database-for-apache-hadoop]({% post_url 2015-05-19-impala-native-analytic-database-for-apache-hadoop %})

1. [2015-05-21-apache-getting-started-with-solr]({% post_url 2015-05-21-apache-getting-started-with-solr %})

1. [2015-05-24-apache-flume-getting-started]({% post_url 2015-05-24-apache-flume-getting-started %})

1. [2015-05-25-apache-hive-getting-started]({% post_url 2015-05-25-apache-hive-getting-started %})

1. [2015-05-25-apache-hive-on-mac-osx-yosemite]({% post_url 2015-05-25-apache-hive-on-mac-osx-yosemite %})

1. [2015-05-25-apache-sqoop-on-mac-osx-yosemite]({% post_url 2015-05-25-apache-sqoop-on-mac-osx-yosemite %})

1. [2015-05-26-apache-hbase-getting-started]({% post_url 2015-05-26-apache-hbase-getting-started %})

1. [2015-05-26-apache-sqoop-getting-started]({% post_url 2015-05-26-apache-sqoop-getting-started %})

1. [2015-05-26-apache-zookeeper-getting-started]({% post_url 2015-05-26-apache-zookeeper-getting-started %})
