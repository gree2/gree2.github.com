---
layout: post
title: "getting started cocoapods"
description: ""
category: [ios]
tags: [xcode, cocoapods]
---
{% include JB/setup %}

### what is cocoapods

[CocoaPods](http://guides.cocoapods.org/using/getting-started.html) manages library dependencies for your Xcode projects

### installation

CocoaPods is build with Ruby

    $ sudo gem install cocoapods

sudo-less installation

* passing `--user-install` flag to `gem install`
* configuring RubyGems env

create or edit `.profile` in your home directory

    export GEM_HOME=$HOME/.gem
    export PATH=$GEM_HOME/bin:$PATH

updating cocoapods

    $ sudo gem install cocoapods
    # to update cocoapods you simply install the gem again

    $ sudo gem install cocoapods --pre
    # for a pre-release version

### using cocoapods

1. create a `Podfile` add your dependencies


    pod 'AFNetworking', '~>2.0'

    pod 'ObjectiveSugar', '~>0.5'

2. run in your project directory


    $ pod install


3. open `xcworkspace` and build


    your_app.xcworkspace

### creating a new project with cocoapods

    1. create a new project
    1. in terminal `$ cd` into your project dir
    1. create a Podfile by running `$ touch Podfile`
    1. add first line `platform :ios, '6.0'`
    1. add a cocoapod by specifying `pod 'ObjectiveSugar'`
    1. save Podfile
    1. run `$ pod install`
    1. open 'your_app.xcworkspace'

### Podfile.lock

    this file keeps track of what version of a pod is installed
    if dependency is updated on the Podfile
    or `pod update` is called
    pods in Podfile.lock will update

### [stackoverflow](http://stackoverflow.com/questions/26990057/cocoapods-commands-fail-due-to-no-such-file-or-directory-dir-initialize-us)

    # /usr/local/rvm/rubies/ruby-2.1.0/lib/ruby/2.1.0/pathname.rb:422:in `open': No such file or directory ...

    run $ pod setup before $ pod install