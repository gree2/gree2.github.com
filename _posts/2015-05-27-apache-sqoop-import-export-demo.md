---
layout: post
title: "apache sqoop import export demo"
description: ""
category: [apache]
tags: [sqoop, import, export, mysql, sqlserver, db2, oracle, avro, csv, teradata, postgresql, netezza]
---
{% include JB/setup %}


### import

1. import table `employee`

    1. --table tablename

            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee

            $ mysql -u root -e "show databases;"

            $ mysql -u root sqoop -e "show tables;"

    1. --target-dir

            absolute path => /usr/cloudera/mysqldata/employee
            relative path => mysqldata/employee

            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee \
            --target-dir /usr/cloudera/mysqldata/employee

            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee \
            --target-dir mysqldata/employee

    1. --warehouse-dir

            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee \
            --warehouse-dir mysqldata -m 1

    1. -m integer

            default is 4
            will generate 4 files on hdfs
            part-m-00000 ... part-m-00003

            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee \
            --target-dir mysqldata/employee -m 1


    1. --where "filter"

            --where "pk between 2 and 4"
    
    1. --check-column pkcol

    1. --incremental append | last_modified

    1. --last-value integer

            # 1. list database `sqoop` table `employee` data
            $ mysql -u root sqoop -e "select * from employee;"
            emp_id, name
            1,      emp_name1
            2,      emp_name2
            3,      emp_name3
            4,      emp_name4
            5,      emp_name5
            6,      emp_name6
            7,      emp_name7
            8,      emp_name8
            9,      emp_name9

            # 2. import
            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee \
            --warehouse-dir mysqldata -m 1

            # 3. insert new data into table `employee`
            $ mysql -u root sqoop -e "insert into table employee values (10, 'emp_name10'), (11, 'emp_name11'), (12, 'emp_name12')"

            # 4. list database `sqoop` table `employee` data
            $ mysql -u root sqoop -e "select * from employee;"
            emp_id, name
            1,      emp_name1
            ...
            9,      emp_name9
            10,     emp_name10
            11,     emp_name11
            12,     emp_name12

            # 5. import `10-12` row into hdfs
            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee \
            --warehouse-dir mysqldata -m 1 \
            --check-column emp_id \
            --incremental append \
            --last-value 9

    1. --options-file filename

            $ pico import.txt
            import
            --connect
            jdbc:mysql://localhost:3306/sqoop
            --username
            sqoop
            -P

            $ sqoop --options-file import.txt --table employee -m 1

1. import data to `hive`

    1. multi-command (all null value data)

            $ sqoop --options-file import.txt --table employee \
            --warehouse-dir sqoopdata -m 1

            $ hive
            hive> show tables;
            hive> desc emps;

            $ sqoop create-hive-table --connect jdbc:mysql://localhost/sqoop \
            --username root --table employee --hive-table emps

            $ hive
            hive> select * from emps;
            # no data
            hive> load data inpath '/usr/cloudera/sqoopdata/employee/part-m-00000' into table emps;
            hive> select * from emps;
            # all null value data
            hive> drop table emps;
            # will also delete `part-m-00000` on hdfs

    1. multi-command (correct data)

            $ sqoop --options-file import.txt --table employee \
            --warehouse-dir sqoopdata -m 1

            $ hive
            hive> show tables;
            hive> desc emps;

            $ sqoop create-hive-table --connect jdbc:mysql://localhost/sqoop \
            --username root --table employee --hive-table emps \
            --mysql-delimiters

            $ hive
            hive> select * from emps;
            # no data
            hive> load data inpath '/usr/cloudera/sqoopdata/employee/part-m-00000' into table emps;
            hive> select * from emps;
            # all null value data
            hive> drop table emps;
            # will also delete `part-m-00000` on hdfs

    1. create-hive-table

    1. hive-import

            # equals multi-command (correct data)
            $ sqoop --options-file import.txt --table employee \
            --hive-table emps --hive-import

### export

1. --export-dir

            $ sqoop export --connect jdbc:mysql://localhost/test \
            --username root -P --table student -m 1 \
            --export-dir '/usr/cloudera/sqoopdata/part-m-00000'

1. --options-file

            $ pico export.txt
            export
            --connect
            jdbc:mysql://localhost:3306/test
            --username
            root

            $ sqoop --options-file export.txt -P --table student -m 1 \
            --export-dir '/usr/cloudera/sqoopdata/part-m-00000'

1. --update-key

            $ sqoop --options-file export.txt --table student \
            --export-dir '/usr/cloudera/sqoopdata/student_detail.txt' \
            --update-key stu_id

### eval list-databases list-tables

1. list-databases

            $ sqoop list-databases --connect jdbc:mysql://localhost \
            --username root

1. list-tables

            $ sqoop list-tables    --connect jdbc:mysql://localhost/mysql \
            --username root

1. eval

            $ sqoop eval --connect jdbc:mysql://localhost/mysql \
            --username root --query "select * from user;"

### sqlserver

1. list-databases

            $ sqoop list-databases --connect jdbc:sqlserver://192.168.100.100:1433 \
            --username sa --password as

1. import

            $ sqoop import --connect jdbc:sqlserver://192.168.100.100:1433;database=demo \
            --username sa --password as --hive-import -m 1 \
            --columns "Name, Year, Rating"

            # if table has no primary key
            # 1. use `--split-by`
            # 2. sequential import with `-m 1`

1. export

            $ sqoop export --connect jdbc:sqlserver://192.168.100.100:1433;database=demo \
            --username sa --password as --table movie \
            --export-dir /usr/cloudera/sqoopdata/part-m-00000 \
            --import-fields-terminated-by '\t'

### cookbook

1. [import](https://cwiki.apache.org/confluence/display/SQOOP/Import+Cookbook)

    1. importing a small table from `db2` (using one map process)

            $ sqoop import --driver com.ibm.db2.jcc.DB2Driver --connect jdbc:db2://db2host:50000/DBNAME \
            --username USER --password passw0rd --table MYTABLE -m 1

    1. import from `oracle` to `avro file` (using 6 map processes)

            # Note: User name and table name must be ALL CAPS
            # You'll get exceptions about lack of columns or non-existent table otherwise.

            $ sqoop import --connect "jdbc:oracle:thin:@(description=(address=(protocol=tcp)(host=orahost)(port=1521))(connect_data=(service_name=myser)))" \
            --username SCOTT --password tiger --table MYTABLE -m 6 \
            --as-avrodatafile --warehouse-dir /staging/sqoop/MYTABLE

    1. sqoop import from `mssql`

            $ sqoop import --driver com.microsoft.sqlserver.jdbc.SQLServerDriver \
            --table MYTABLE --connect 'jdbc:sqlserver://MSSQLSeverHost;database=DBNAME;username=USER;password=PASSWORD'

1. [export](https://cwiki.apache.org/confluence/display/SQOOP/Export+Cookbook)

    1. exporting data from `csv file` to `teradata`

            # note: teradata connector is required.
            $ sqoop export --connect jdbc:teradata://terahost/DATABASE=MYDB \    
            --username myuser --password mypass \ 
            --export-dir /user/hive/warehouse/mydb.db/data_stg \ 
            --table MYTABLE --input-fields-terminated-by ','  
 
    1. merging data into `oracle` with `oraoop`
            
            $ sqoop export -Doraoop.export.merge=true -Doraoop.update.key.extra.columns="period_start_time" \
            -D mapred.map.max.attempts=1 \
            --connect ... --username user --password pass --table table2 \
            --export-dir /user/hive/warehouse/tables/mytable \
            --verbose -m 16 --update-key co_gid
 
    1. exporting a `hive table` stored in a custom schema to `postgresql`

            # sqoop does not have a notion of custom schemas
            # you need to specify the parameter 
            # --schema with a schema name if your table is stored in a different schema
            # please note that the hive schema must have the same name as the postgres one
            # the --schema parameter must be separated from the rest of the parameters
            # with an extra set of dashes (i.e. -- )
            # and the --schema parameter must come last

            $ sqoop export --connect jdbc:postgresql://postgresql.example.com/database \
            --username sqoop --password sqoop \
            --table cities --export-dir cities --schema us
 
    1. exporting `hdfs dir` to `netezza`

            $ sqoop export --direct --connect jdbc:netezza://NetezzaHost:5480/MyTable --username USER \ 
            --password-file <pwd file location> --batch --export-dir <HDFS dir to export> --table NZTable \ 
            --input-fields-terminated-by '\0001' --input-null-string '\\N' --input-null-non-string '\\N’ 