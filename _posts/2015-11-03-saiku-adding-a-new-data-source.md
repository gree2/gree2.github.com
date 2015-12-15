---
layout: post
title: "saiku adding a new data source"
description: ""
category: [bi]
tags: [saiku, olap, mondrian, mysql]
---
{% include JB/setup %}


### saiku adding a new data source

1. import schema and configure it to use mysql database connection

    1. create a new file

            $ cd /Applications/Saiku Enterprise/server/tomcat/webapps/saiku/WEB-INF/classes/saiku-datasources
            $ pico steelwheels
            type=OLAP
            name=steelwheels
            driver=mondrian.olap4j.MondrianOlap4jDriver
            location=jdbc:mondrian:Jdbc=jdbc:mysql://localhost/sampledata;Catalog=../webapps/saiku/steelwheels/steelwheels.mondrian.xml;JdbcDrivers=com.mysql.jdbc.Driver;
            username=dbuser
            password=password

    1. description

            # told saiku the new name of connection is `steelwheels`
            # told saiku the location of the database and `mondrian` schema
            # create a `steelwheels` folder under the `webapps/` directory

    1. add the jdbc connector jar file

            $ cd /Applications/Saiku Enterprise/server/tomcat/webapps/saiku/WEB-INF/lib
            # copy `mysql-connector-java-5.1.35-bin.jar` to here 

    1. after installed schema definition

            $ cd /Applications/Saiku Enterprise/server
            $ sh start-saiku.sh 
