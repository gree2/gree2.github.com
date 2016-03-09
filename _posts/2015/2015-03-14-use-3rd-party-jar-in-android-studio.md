---
layout: post
title: "use 3rd party jar in android studio"
description: ""
category: [android]
tags: [jar, lib]
---
{% include JB/setup %}


### solution 1 use jar file

1. download `volley.jar` file
1. change to `project` view
1. drag `volley.jar` to `libs` folder
1. right click `volley.jar`
1. click `add as library`

### solution 2 use gradle compile

1. open your module's `build.gradle`
1. add next line to `dependencies`
    * compile 'com.umeng.analytics:analytics:latest.integration'
1. sync your project