---
layout: post
title: "mac font locations and their purposes"
description: ""
category: [mac]
tags: [font, location, purpose]
---
{% include JB/setup %}

### mac basics: font book app

[reference support.apple.com](https://support.apple.com/en-us/HT201749)

### font use - 1 user

* [font folder location](https://support.apple.com/en-us/HT201722)

            ~/Library/Fonts/

    * each user has complete control over the fonts installed in their home

    * these fonts are available to that user when he or she logged into the computer

    * fonts installed here are not available to all users of the computer

### font use - 2 local

* [font folder location](https://support.apple.com/en-us/HT201722)

            /Library/Fonts/

    * any local user of the computer can use fonts installed in this folder

    * Mac OS X dose not require these additional fonts for system operation

    * an admin user can modify the contents of this folder

    * this is the recommended location for fonts that are shared among applications

### font use - 3 network

* [font folder location](https://support.apple.com/en-us/HT201722)

            /Network/Library/Fonts/

    * the network folder is for fonts shared among all users of a local area network

    * this feature is normally used on network file servers, under the control of a network admin

### font use - 4 system

* [font folder location](https://support.apple.com/en-us/HT201722)

            /System/Library/Fonts/

    * Max OS X requires fonts in this folder for system use and displays

    * they should not be manually altered or removed

### font use - 5 classic

* [font folder location](https://support.apple.com/en-us/HT201722)

            /System Folder/Fonts

    * this folder contains fonts used by the Classic environment (Mac OS X v10.4 or earlier only)

    * if more than one Mac OS 9.1 System Folder is present, only fonts in the System Folder selected in the Classic pane of System Preferences are used

    * Classic apps can access only these fonts, not those sotred elsewhere

    * conversely Mac OS X apps can use these fonts, even when the Classic environment is not active