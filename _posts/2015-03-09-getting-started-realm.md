---
layout: post
title: "getting started realm"
description: ""
category: [swift]
tags: [realm]
---
{% include JB/setup %}

### installation

1. edit Podfile
	add **pod "Realm"** to your app target
	add **pod "Realm/Headers"** to your test target

1. run `pod install`

1. use the `.xcworkspace` file

1. follow these additional step if you use swift

	* download the [latest release](http://static.realm.io/downloads/cocoa/latest) and unzip
	* drag the file `Swift/RLMSupport.swift` into the file navigator of your xcode project

### realm browser

[download zip](http://static.realm.io/downloads/cocoa/latest)

tools > generate demo database