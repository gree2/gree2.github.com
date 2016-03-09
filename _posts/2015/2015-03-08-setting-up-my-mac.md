---
layout: post
title: "setting up my mac"
description: ""
category: [mac]
tags: [ruby, jekyll]
---
{% include JB/setup %}

### setup ruby

    gem sources --remove https://rubygems.org/
    gem sources -a https://ruby.taobao.org/
    gem sources -l
    gem install bundler

### setup jekyll

    gem install jekyll
    jekyll new blog
    cd blog
    jekyll serve

    rake post title="hello world"
    rake page name="about.md"
    rake page name="pages/about.md"
    rake page name="pages/about"

### setup homebrew

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install python-setuptools

### show hide hidden file

    using terminal
    ==============
    $ defaults write com.apple.finder AppleShowAllFiles YES
    $ defaults write com.apple.finder AppleShowAllFiles NO
    hold alt and right click Finder icon click Relaunch

    using alias
    ===========
    $ sudo pico ~/.bash_profile
    alias showfiles='defaults write com.apple.finder AppleShowAllFiles YES; killall Finder /System/Library/CoreServices/Finder.app'
    alias hidefiles='defaults write com.apple.finder AppleShowAllFiles NO; killall Finder /System/Library/CoreServices/Finder.app'
    $ source ~/.bash_profile
    $ showfiles
    $ hidefiles

### setup sublime text

    sublime text 3 package control
    ==============================
    import urllib.request,os,hashlib; h = 'eb2297e1a458f27d836c04bb0cbaf282' + 'd0e7a3098092775ccb37ca9d6b2e4b7d'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

    sublime text 2 package control
    ==============================
    import urllib2,os,hashlib; h = 'eb2297e1a458f27d836c04bb0cbaf282' + 'd0e7a3098092775ccb37ca9d6b2e4b7d'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')

### setup android studio

1. disable first run

            bin/idea.properties
            ===================
            disable.android.first.run=true

1. android sdk rar package

    * [download for windows x64](http://yunpan.cn/cKpvzwbSdUAcZ) code 8750

            default location
            C:\Users\username\AppData\Local\Android\sdk

    * [download for Mac OSX](http://yunpan.cn/cVNik48h2iRtX) code 7cfa

            default location
            /Users/username/Library/Android/android-sdk-macosx

### lock screen

1. `system preferences`
1. `security & privacy` under `general` tab
1. `require password immediately` after sleep or screen saver begins
1. `control+shift+power`

### ssh localhost

1. `system preferences`
1. `sharing`
1. `remote login`
    
### open current foler in finder form terminal

            $ open .