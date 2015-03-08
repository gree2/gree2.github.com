---
layout: post
title: "cannot launch android sdk manager"
description: ""
category: [android]
tags: [android sdk manager, android.bat, find_java.bat]
---
{% include JB/setup %}

### error

cannot launch **SDK Manager.exe**
    
try to run **sdk/tools/android.bat** but you got this error

    '-v' is not recognized as an internal or external command,
    operable program or batch file.
    ERROR: SWT folder '' does not exist.
    Please set ANDROID_SWT to point to the folder containing swt.jar for your platform.

### solution

1. find someone who can run **SDK Manager.exe**
2. copy his or hers **sdk/tools/lib/find_java.bat**
3. paste this bat file to your **sdk/tools/lib/**
4. run **SDK Manager.exe**
