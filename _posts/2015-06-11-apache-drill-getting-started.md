---
layout: post
title: "apache drill getting started"
description: ""
category: [bigdata]
tags: [drill, brew]
---
{% include JB/setup %}


### info


### install

1. use brew

            $ brew install apache-drill
            ==> Downloading https://www.apache.org/dyn/closer.cgi?path=drill/drill-1.0.0/apa
            ==> Best Mirror http://mirrors.hust.edu.cn/apache/drill/drill-1.0.0/apache-drill
            ######################################################################## 100.0%
            🍺  /usr/local/Cellar/apache-drill/1.0.0: 253 files, 163M, built in 69.4 minutes

### demo

1. [drill in 10 minutes](https://drill.apache.org/docs/drill-in-10-minutes/)

    1. launch drill in embedded mode

            $ drill-embedded 
            Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=512M; support was removed in 8.0
            Jun 11, 2015 4:17:09 PM org.glassfish.jersey.server.ApplicationHandler initialize
            INFO: Initiating Jersey application, version Jersey: 2.8 2014-04-29 01:25:26...
            apache drill 1.0.0 
            "a drill is a terrible thing to waste"
            0: jdbc:drill:zk=local> 

    1. stop drill

            > !quit

    1. query sample data

            0: jdbc:drill:zk=local> select * from cp.`employee.json` limit 3;
            +--------------+------------------+-------------+------------+--------------+---------------------+-----------+----------------+-------------+------------------------+----------+----------------+------------------+-----------------+---------+--------------------+
            | employee_id  |    full_name     | first_name  | last_name  | position_id  |   position_title    | store_id  | department_id  | birth_date  |       hire_date        |  salary  | supervisor_id  | education_level  | marital_status  | gender  |  management_role   |
            +--------------+------------------+-------------+------------+--------------+---------------------+-----------+----------------+-------------+------------------------+----------+----------------+------------------+-----------------+---------+--------------------+
            | 1            | Sheri Nowmer     | Sheri       | Nowmer     | 1            | President           | 0         | 1              | 1961-08-26  | 1994-12-01 00:00:00.0  | 80000.0  | 0              | Graduate Degree  | S               | F       | Senior Management  |
            | 2            | Derrick Whelply  | Derrick     | Whelply    | 2            | VP Country Manager  | 0         | 1              | 1915-07-03  | 1994-12-01 00:00:00.0  | 40000.0  | 1              | Graduate Degree  | M               | M       | Senior Management  |
            | 4            | Michael Spence   | Michael     | Spence     | 2            | VP Country Manager  | 0         | 1              | 1969-06-20  | 1998-01-01 00:00:00.0  | 40000.0  | 1              | Graduate Degree  | S               | M       | Senior Management  |
            +--------------+------------------+-------------+------------+--------------+---------------------+-----------+----------------+-------------+------------------------+----------+----------------+------------------+-----------------+---------+--------------------+
            3 rows selected (1.871 seconds)

    1. query a parquet file

            0: jdbc:drill:zk=local> select * from dfs.`/usr/local/apache-drill/libexec/sample-data/region.parquet` limit 3;
            SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
            SLF4J: Defaulting to no-operation (NOP) logger implementation
            SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
            +--------------+----------+-----------------------+
            | R_REGIONKEY  |  R_NAME  |       R_COMMENT       |
            +--------------+----------+-----------------------+
            | 0            | AFRICA   | lar deposits. blithe  |
            | 1            | AMERICA  | hs use ironic, even   |
            | 2            | ASIA     | ges. thinly even pin  |
            +--------------+----------+-----------------------+
            3 rows selected (0.254 seconds)

            0: jdbc:drill:zk=local> select * from dfs.`/usr/local/apache-drill/libexec/sample-data/nation.parquet`;
            +--------------+-----------------+--------------+-----------------------+
            | N_NATIONKEY  |     N_NAME      | N_REGIONKEY  |       N_COMMENT       |
            +--------------+-----------------+--------------+-----------------------+
            | 0            | ALGERIA         | 0            |  haggle. carefully f  |
            | 1            | ARGENTINA       | 1            | al foxes promise sly  |
            | 2            | BRAZIL          | 1            | y alongside of the p  |
            | 3            | CANADA          | 1            | eas hang ironic, sil  |
            | 4            | EGYPT           | 4            | y above the carefull  |
            | 5            | ETHIOPIA        | 0            | ven packages wake qu  |
            | 6            | FRANCE          | 3            | refully final reques  |
            | 7            | GERMANY         | 3            | l platelets. regular  |
            | 8            | INDIA           | 2            | ss excuses cajole sl  |
            | 9            | INDONESIA       | 2            |  slyly express asymp  |
            | 10           | IRAN            | 4            | efully alongside of   |
            | 11           | IRAQ            | 4            | nic deposits boost a  |
            | 12           | JAPAN           | 2            | ously. final, expres  |
            | 13           | JORDAN          | 4            | ic deposits are blit  |
            | 14           | KENYA           | 0            |  pending excuses hag  |
            | 15           | MOROCCO         | 0            | rns. blithely bold c  |
            | 16           | MOZAMBIQUE      | 0            | s. ironic, unusual a  |
            | 17           | PERU            | 1            | platelets. blithely   |
            | 18           | CHINA           | 2            | c dependencies. furi  |
            | 19           | ROMANIA         | 3            | ular asymptotes are   |
            | 20           | SAUDI ARABIA    | 4            | ts. silent requests   |
            | 21           | VIETNAM         | 2            | hely enticingly expr  |
            | 22           | RUSSIA          | 3            |  requests against th  |
            | 23           | UNITED KINGDOM  | 3            | eans boost carefully  |
            | 24           | UNITED STATES   | 1            | y final packages. sl  |
            +--------------+-----------------+--------------+-----------------------+
            25 rows selected (0.102 seconds)

