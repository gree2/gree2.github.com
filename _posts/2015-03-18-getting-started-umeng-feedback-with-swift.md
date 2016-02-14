---
layout: post
title: "getting started umeng feedback with swift"
description: ""
category: [android]
tags: [feedback, swift, umeng]
---
{% include JB/setup %}

1. [download iOS SDK][1]
1. unzip `UMFeedback_iOS_2.1.zip`
1. drag `UMengFeedback_SDK_2.1` into file navigator and `copy items if needed`
1. in `Build Phases` in `Link Binary With Libraries`
    * add `AVFoundation.framework`
1. in `AppDelegate.swift`
    
    * find `func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {`

    * add `UMFeedback.setAppkey(appKey)`

1. present feedbackViewController
	* `self.presentViewController(UMFeedback.feedbackViewController(), animated: true){ () -> Void in }`

[1]: http://dev.umeng.com/files/download/UMFeedback_iOS_2.1.zip