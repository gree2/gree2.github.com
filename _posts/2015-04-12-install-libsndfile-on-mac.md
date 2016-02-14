---
layout: post
title: "install libsndfile on mac"
description: ""
category: [setup]
tags: [libsndfile, mac, install]
---
{% include JB/setup %}

### install

1. install from source [reference](http://stackoverflow.com/questions/13999790/python-audiolab-install-unable-to-install-or-find-libsndfile-on-mac-osx)

    1. download the [latest source](http://www.mega-nerd.com/libsndfile/files/libsndfile-1.0.25.tar.gz) from [www.mega-nerd.com](http://www.mega-nerd.com/libsndfile/)

    1. unpack it

    1. `cd` the directory

    1. configure it

            $ ./configure

    1. build it

            $ make -j8

    1. install it

            $ sudo make -j8 install

    1. delete the directory you unpacked

            $ rm -rf libsndfile-1.0.25/
            # you may want to keep it for easy uninstallation

1. use homebrew

    1. `$ brew install libsndfile`

            $ brew install libsndfile

            ==> Installing dependencies for libsndfile: flac, libogg, libvorbis
            ==> Installing libsndfile dependency: flac
            ==> Downloading https://homebrew.bintray.com/bottles/flac-1.3.1.yosemite.bottle.tar.gz
            ######################################################################## 100.0%
            ==> Pouring flac-1.3.1.yosemite.bottle.tar.gz
            Error: The `brew link` step did not complete successfully
            The formula built, but is not symlinked into /usr/local
            Could not symlink lib/libFLAC++.6.dylib
            /usr/local/lib is not writable.

            You can try again using:
              brew link flac
            ==> Summary
            🍺  /usr/local/Cellar/flac/1.3.1: 50 files, 2.4M
            ==> Installing libsndfile dependency: libogg
            ==> Downloading https://homebrew.bintray.com/bottles/libogg-1.3.2.yosemite.bottle.tar.gz
            ######################################################################## 100.0%
            ==> Pouring libogg-1.3.2.yosemite.bottle.tar.gz
            Error: The `brew link` step did not complete successfully
            The formula built, but is not symlinked into /usr/local
            Could not symlink lib/libogg.0.dylib
            /usr/local/lib is not writable.

            You can try again using:
              brew link libogg
            ==> Summary
            🍺  /usr/local/Cellar/libogg/1.3.2: 95 files, 672K
            ==> Installing libsndfile dependency: libvorbis
            ==> Downloading https://homebrew.bintray.com/bottles/libvorbis-1.3.5.yosemite.bottle.tar.gz
            ######################################################################## 100.0%
            ==> Pouring libvorbis-1.3.5.yosemite.bottle.tar.gz
            Error: The `brew link` step did not complete successfully
            The formula built, but is not symlinked into /usr/local
            Could not symlink lib/libvorbis.0.dylib
            /usr/local/lib is not writable.

            You can try again using:
              brew link libvorbis
            ==> Summary
            🍺  /usr/local/Cellar/libvorbis/1.3.5: 155 files, 2.6M
            ==> Installing libsndfile
            ==> Downloading https://homebrew.bintray.com/bottles/libsndfile-1.0.25.yosemite.bottle.1.tar.gz
            ######################################################################## 100.0%
            ==> Pouring libsndfile-1.0.25.yosemite.bottle.1.tar.gz
            Error: The `brew link` step did not complete successfully
            The formula built, but is not symlinked into /usr/local
            Could not symlink lib/libsndfile.1.dylib
            /usr/local/lib is not writable.

            You can try again using:
              brew link libsndfile
            ==> Summary
            🍺  /usr/local/Cellar/libsndfile/1.0.25: 47 files, 1.8M

    1. `$ sudo !!`

            $ sudo !!
            sudo brew install libsndfile
            Password:
            Error: Cowardly refusing to `sudo brew install`
            You can use brew with sudo, but only if the brew executable is owned by root.
            However, this is both not recommended and completely unsupported so do so at
            your own risk.

    1. `$ brew install libsndfile`       

            $ brew install libsndfile
            Warning: libsndfile-1.0.25 already installed, it's just not linked
    
    1. `$ brew uninstall libsndfile`

            $ brew uninstall libsndfile
            Uninstalling /usr/local/Cellar/libsndfile/1.0.25...
    
    1. `$ sudo chown -R hqlgree2 /usr/local` [reference](http://stackoverflow.com/questions/11237048/homebrew-symlink-error)

            $ sudo chown -R hqlgree2 /usr/local
    
    1. `$ brew install libsndfile`

            $ brew install libsndfile
            Error: You must `brew link flac libogg libvorbis' before libsndfile can be installed
    
    1. `$ brew link flac libogg libvorbis`

            $ brew link flac libogg libvorbis
            Linking /usr/local/Cellar/flac/1.3.1... 17 symlinks created
            Linking /usr/local/Cellar/libogg/1.3.2... 7 symlinks created
            Linking /usr/local/Cellar/libvorbis/1.3.5... 15 symlinks created
    
    1. `$ brew install libsndfile`

            $ brew install libsndfile
            ==> Downloading https://homebrew.bintray.com/bottles/libsndfile-1.0.25.yosemite.bottle.1.tar.gz
            Already downloaded: /Library/Caches/Homebrew/libsndfile-1.0.25.yosemite.bottle.1.tar.gz
            ==> Pouring libsndfile-1.0.25.yosemite.bottle.1.tar.gz
            🍺  /usr/local/Cellar/libsndfile/1.0.25: 47 files, 1.8M
