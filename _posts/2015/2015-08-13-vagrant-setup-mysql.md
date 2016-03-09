---
layout: post
title: "vagrant setup mysql"
description: ""
category: [auto]
tags: [vagrant, mysql, python, mysql-server, mysql-client, python-mysqldb]
---
{% include JB/setup %}


### steps

1. playbook from [install mysql with ansible on ubuntu](http://stackoverflow.com/questions/26597926/install-mysql-with-ansible-on-ubuntu)

### fixed

1. the python mysqldb module is required

    1. errors

            TASK: [mysql | update mysql password] ***************************************** 
            <192.168.100.100> REMOTE_MODULE mysql_user priv='*.*:ALL,GRANT' password=VALUE_HIDDEN login_password=VALUE_HIDDEN host=singlenode login_user=root name=root
            failed: [singlenode] => (item=singlenode) => {"failed": true, "item": "singlenode"}
            msg: the python mysqldb module is required
            <192.168.100.100> REMOTE_MODULE mysql_user priv='*.*:ALL,GRANT' password=VALUE_HIDDEN login_password=VALUE_HIDDEN host=127.0.0.1 login_user=root name=root
            failed: [singlenode] => (item=127.0.0.1) => {"failed": true, "item": "127.0.0.1"}
            msg: the python mysqldb module is required
            <192.168.100.100> REMOTE_MODULE mysql_user priv='*.*:ALL,GRANT' password=VALUE_HIDDEN login_password=VALUE_HIDDEN host=::1 login_user=root name=root
            failed: [singlenode] => (item=::1) => {"failed": true, "item": "::1"}
            msg: the python mysqldb module is required

    1. fixed

            put `python-mysqldb` into task `apt`'s `with_items`
