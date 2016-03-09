---
layout: post
title: "mondrian installation"
description: ""
category: [bigdata]
tags: [mondrian]
---
{% include JB/setup %}


### [solution 1](http://mondrian.pentaho.com/documentation/installation.php)

1. install java sdk

1. download the latest binary release [mondrian-version.zip](http://sourceforge.net/projects/mondrian)

1. for non-embedded distribution setup foodmart dataset

    1. create `foodmart` database and `foodmart` user

            $ mysql -u root -p
            mysql> create user 'foodmart'@'localhost' identified by 'foodmart';
            mysql> grant all privileges on *.* to 'foodmart'@'localhost' identified by 'foodmart';
            mysql> quit;

    2. load the data

            $ java -cp "/mondrian/lib/mondrian.jar:/mondrian/lib/log4j.jar:/mondrian/lib/commons-logging.jar:/mondrian/lib/eigenbase-xom.jar:/mondrian/lib/eigenbase-resgen.jar:/mondrian/lib/eigenbase-properties.jar:/usr/local/mysql/mysql-connector-java-5.0.5-bin.jar"
            mondrian.test.loader.MondrianFoodMartLoader
            -verbose -tables -data -indexes
            -jdbcDrivers=com.mysql.jdbc.Driver
            -inputFile=/mondrian/demo/FoodMartCreateData.sql 
            -outputJdbcURL="jdbc:mysql://localhost/foodmart?user=foodmart&password=foodmart"

1. setup and start web app

### [solution 2](http://www.garrettpatterson.com/2013/08/02/setup-mondrian-on-mac-osx/)

1. steps

    1. clone git repo

            $ git clone https://github.com/pentaho/mondrian.git

    1. create a file at the root of the project

            $ cd mondrian
            $ pico mondrian.properties
            mondrian.foodmart.jdbcURL=jdbc:mysql://localhost/foodmart
            mondrian.foodmart.jdbcUser=foodmart
            mondrian.foodmart.jdbcPassword=foodmart
            mondrian.jdbcDrivers=com.mysql.jdbc.Driver
            driver.classpath=/Users/foo/mysql-connector-java.jar

    1. compile it

            $ ant jar

            # if you run into issues about JAVA_HOME
            # pop open `buildOnJdk.sh`
            # look for a section add your own paths

    1. load the demo database

            $ ant load-foodmart

    1. make your changes and run some tests

            $ ant junit-main

1. references

    1. [git workflow](https://github.com/pentaho/mondrian/wiki/Forking-and-Contributing-to-Mondrian)

    1. [dev guide](http://mondrian.pentaho.com/documentation/developers_guide.php)

    1. for easy deployment use [mondrian-xmla-spike](https://github.com/ThoughtWorksInc/mondrian-xmla-spike) from `Thought Works`

            # it will fire up a mondrian xmla service
            # which you can access with all xmla compliant tools
            # i suggest using warehouse explorer