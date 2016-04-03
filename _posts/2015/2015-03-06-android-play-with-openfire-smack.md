---
layout: post
title: "android play with openfire smack"
description: ""
category: [android]
tags: [android, openfire, smack]
---
{% include JB/setup %}

### prerequisite

1. download and install [openfire](http://www.igniterealtime.org/projects/openfire/)
2. download and install [spark](http://www.igniterealtime.org/downloads/index.jsp)
3. download the [XmppDemo](https://github.com/gree2/XmppDemo)

### step 1.1 install openfire

1. unzip **openfire_3_9_3.zip** to some path
2. cd to **/some/path/to/openfire/bin**
3. run **openfire.exe**
4. click **Launch Admin** will open you browser

### step 1.2 setup openfire

1. on **welcome to setup** page choose language **english(en)**
2. on **server settings** page
    * domain: **your computer name**
    * admin console port: **9090**
    * secure admin console port: **9091**
3. on **database settings** page choose **embedded database**
4. on **profile settings** page choose **default**
5. on **administrator account** page
    * admin email address: **admin**
    * new password: **admin**
    * confirm password: **admin**
6. on **setup complete** you can click **login to the admin console**

### step 1.3 add a test user

1. change to **user/group** tab click **create new user**
2. on **create user** page
    * username: **gree2**
    * name: **gree2**
    * email: ****
    * password: **gree2**
    * confirm password: **gree2**
3. click **create user**
4. repeate step 2 create **test**

### step 2.1 install spark

1. run **spark_2_6_3.exe**
2. follow the wizard

### step 2.2 spark login

1. run your spark program
2. input login info
* username: **gree2**
* password: **gree2**
* server: 192.168.100.32
3. click **login**

### step 3 run android app

1. clone the repo
2. open with **android studio**
3. just run it
4. spark will receive a message: **This message is from Android**
