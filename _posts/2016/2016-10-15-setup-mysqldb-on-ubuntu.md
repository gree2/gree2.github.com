---
layout: post
title: "setup mysqldb on ubuntu"
description: ""
category: [python]
tags: [python-mysqldb, mysql-python, apt, pip, libmysqlclient-dev]
---
{% include JB/setup %}


### steps

1. install python-mysqldb

        $ sudo apt install python-mysqldb

1. install mysql-python

        $ pip install mysql-python

### fixed

1. mysql_config not found

    1. error

            Collecting mysql-python
              Downloading MySQL-python-1.2.5.zip (108kB)
                100% |████████████████████████████████| 112kB 176kB/s 
                Complete output from command python setup.py egg_info:
                sh: 1: mysql_config: not found
                Traceback (most recent call last):
                  File "<string>", line 1, in <module>
                  File "/tmp/pip-build-Wp57LT/mysql-python/setup.py", line 17, in <module>
                    metadata, options = get_config()
                  File "/tmp/pip-build-Wp57LT/mysql-python/setup_posix.py", line 43, in get_config
                    libs = mysql_config("libs_r")
                  File "/tmp/pip-build-Wp57LT/mysql-python/setup_posix.py", line 25, in mysql_config
                    raise EnvironmentError("%s not found" % (mysql_config.path,))
                EnvironmentError: mysql_config not found

    1. fixed

            $ sudo apt-get install libmysqlclient-dev

1. No module named MySQLdb

    1. error

                 62     def dbapi(cls):
                 63         # is overridden when pymysql is used
            ---> 64         return __import__('MySQLdb')
                 65 
                 66 

            ImportError: No module named MySQLdb

    1. fixed

            $ pip install mysql-python

### references

1. [no-module-named-mysqldb](http://stackoverflow.com/questions/454854/no-module-named-mysqldb)
