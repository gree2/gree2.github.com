---
layout: post
title: "getting started realm for ios"
description: ""
category: [ios]
tags: [realm, swift, pod]
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

> for now this solution didn't work for me

### manual installation

1. download the [latest release](http://static.realm.io/downloads/cocoa/latest)
1. drag **Realm.framework** from the **ios/** dir into File Navigator checking **Copy items if needs**
1. in **Build Phased** tab add **libc++.dylib** under **Link Binary with Libraries**
1. drag **Swift/RLMSupport.swift** into File Navigator checking **Copy items if needs**

> this solution did work for me

### realm browser

[download zip](http://static.realm.io/downloads/cocoa/latest)

tools > generate demo database