---
layout: post
title: "eastern pioneer bus ios"
description: ""
category: [ios]
tags: [swift]
---
{% include JB/setup %}


### tech

1. networking - [Alamofire](https://github.com/Alamofire/Alamofire)
1. database - [Realm](http://realm.io/)
3. html parse - [HTMLDocument](https://github.com/stklieme/HTMLDocument)

### tech 1 setup alamofire

embedded framework

1. add alamofire as a `submodule`
	* open `terminal`
	* `cd` to top-level project dir
	* `git submodule add https://github.com/Alamofire/Alamofire.git`
1. open `Alamofire` folder drag `Alamofire.xcodeproj` into `file navigator`
1. ensure `Deployment Target` of `Alamofire.framework` matches application target
1. open `Build phases`
	* select `Target Dependencies` group
	* add `Alamofire.framework`
1. click `+` 
	* select `New Copy File Phases`
	* rename this new phases to `Copy Frameworks`
	* set the `Destination` to `Frameworks`
	* add `Alamofire.framework`

### tech 2 setup Realm

1. download the [latest release](http://static.realm.io/downloads/cocoa/latest)
1. drag `ios/Realm.framework`
	* into `file navigator`
	* checking `Copy items if needs`
1. in `Build Phased` tab under `Link Binary with Libraries`
	* add `libc++.dylib`
1. drag `Swift/RLMSupport.swift`
	* into `file navigator`
	* checking `Copy items if needs`

### tech 3 setup HTMLDocument

1. add class files and the (option) category/extension to your project
1. in `Build Phased` tab under `Link Binary with Libraries`
	* add `libxml2.dylib`
1. in `Build Settings` under `Header Search Pathes`
	* add `$SDKROOT/usr/include/libxml2`
1. in `Build Settings` under `other linker flags`
	* add `-lxml2`
1. add `Bridging-Header.h`
1. in `Build Settings` under `Objective-C Bridging Header`
	* enter `Bridging-Header.h`

### ui

1. drawer - [create-slide-out-navigation-panel-swift](http://www.raywenderlich.com/78568/create-slide-out-navigation-panel-swift)
