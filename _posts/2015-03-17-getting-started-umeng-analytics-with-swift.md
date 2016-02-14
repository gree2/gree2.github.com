---
layout: post
title: "getting started umeng analytics with swift"
description: ""
category: [android]
tags: [analytics, umeng]
---
{% include JB/setup %}

### solution

1. add `#import "mobclick.h"` to your `Bridging-Header.h`
1. drag these files into file navigator
    * `libMobClickLibrary.a`
    * `MobClick.h`
1. in `Build Phases` under `Link Binary With Libraries`
    * add `libz.dylib`
1. add code to your `application didFinishLaunchWithOptions`
    * `MobClick.startWithAppkey("app_key", reportPolicy: BATCH, channelId: "")`
    * `MobClick.setAppVersion(getAppVersion())`
1. add code to your view file

        override func viewWillAppear(animated: Bool) {
            super.viewWillAppear(animated)
            MobClick.beginLogPageView("view_01")
        }

        override func viewWillDisappear(animated: Bool) {
            super.viewWillDisappear(animated)
            MobClick.endLogPageView("view_01")
        }

### getAppVersion()

    func getAppVersion() -> String {
        let dictionary = NSBundle.mainBundle().infoDictionary!
        let version = dictionary["CFBundleShortVersionString"] as String
        let build = dictionary["CFBundleVersion"] as String
        return "\(version).\(build)"
    }