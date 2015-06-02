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

    1. homebrew `formulae` are simple `ruby scripts`

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
