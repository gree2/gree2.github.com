---
layout: post
title: "manually sign and align your apk"
description: ""
category: [android]
tags: [sign, align, ]
---
{% include JB/setup %}

### solution

1. generate a private key using [keytool][1]

        $ keytool -genkeypair -dname "CN=hqlgree2, OU=DA, O=ICSS, L=BeiJing, ST=HaiDian, C=CN" -alias epb -keyalg RSA -keypass password_of_keypass -keystore epb.keystore -storepass password_of_storepass -validity 365


2. compile your app in release mode to obtain an unsigned apk

3. sign your app with your private key using [jarsigner][2]

        $ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -tsa http://timestamp.digicert.com -keystore epb.keystore -storepass password_of_storepass epb.apk epb

4. verify that your apk is signed

        $ jarsigner -verify -verbose -certs epb.apk

5. align the final apk package using [zipalign][3]

        $ zipalign -v 4 epb.apk epb_align.apk

### refer

tool || description
--- || ---
[keytool][1] |-->| Manages a keystore (database) of cryptographic keys, X.509 certificate chains, and trusted certificates.
[jarsigner][2] |-->| Generates signatures for Java ARchive (JAR) files, and verifies the signatures of signed JAR files.
[zipalign][3] |-->| an archive alignment tool that provides important optimization to Android application (.apk) files.


[1]: http://slashdot.org
[2]: http://slashdot.org
[3]: http://slashdot.org