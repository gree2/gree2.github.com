---
layout: post
title: "apache sqoop import export demo"
description: ""
category: [bigdata]
tags: [sqoop, import, export, mysql, sqlserver, db2, oracle, avro, csv, teradata, postgresql, netezza]
---
{% include JB/setup %}


### mysql test data

1. create database table

            $ mysql -u root -p
            mysql> create database sqoop;
            mysql> use sqoop;
            mysql> create table employee(
            ->  emp_id int not null auto_increment,
            ->  emp_name  varchar(32) not null,
            ->  emp_title varchar(32) not null,
            ->  primary key ( emp_id )
            ->);

1. polulate data

            mysql> insert into employee values (1, 'name1', 'dev'), (2, 'name2', 'pm'),
            -> (3, 'name3', 'qa'), (4, 'name4', 'test'), (5, 'name5', 'dev');
            Query OK, 5 rows affected (0.00 sec)
            Records: 5  Duplicates: 0  Warnings: 0

            mysql> select * from employee;
            +--------+----------+-----------+
            | emp_id | emp_name | emp_title |
            +--------+----------+-----------+
            |      1 | name1    | dev       |
            |      2 | name2    | pm        |
            |      3 | name3    | qa        |
            |      4 | name4    | test      |
            |      5 | name5    | dev       |
            +--------+----------+-----------+
            5 rows in set (0.00 sec)

1. grant privileges

    1. user `sqoop`

            mysql> create user 'sqoop'@'localhost' identified by 'sqoop';
            mysql> grant all privileges on *.* to 'sqoop'@'localhost';
            mysql> flush privileges;

    1. user `root`

            mysql> grant all privileges on *.* to 'root'@'%';
            mysql> flush privileges;
            mysql> use mysql;
            mysql> update user set password=password('root') where user='root';
            mysql> flush privileges;

1. execute sql

            $ mysql -u root -e "show databases;"
            $ mysql -u root sqoop -e "show tables;"

### import

1. import table `employee`

    1. --table tablename

            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee

            # 1. <----------------
            # ERROR tool.ImportTool:
            # Encountered IOException running import job:
            # java.io.FileNotFoundException:
            # File does not exist:
            # hdfs://localhost:9000/usr/local/Cellar/sqoop/1.4.5/libexec/lib/paranamer-2.3.jar

            $ hdfs dfs -mkdir -p /usr/local/Cellar/sqoop/
            $ hdfs dfs -put /usr/local/Cellar/sqoop/* /usr/local/Cellar/sqoop/

            # 2. <----------------
            # java.lang.Exception:
            # java.lang.RuntimeException:
            # java.lang.ClassNotFoundException: Class employee not found

            $ sqoop codegen --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee
            # INFO orm.CompilationManager:
            # Writing jar file:
            # /tmp/sqoop-hqlgree2/compile/199c329d20fd26512ff0f9593facdb3e/employee.jar

            $ sqoop import -fs local -jt local -libjars \
            /tmp/sqoop-hqlgree2/compile/199c329d20fd26512ff0f9593facdb3e/employee.jar \
            --connect jdbc:mysql://localhost/sqoop --username sqoop --password sqoop \
            --table employee --target-dir employee
            $ ls
            employee employee.java

            # http://stackoverflow.com/questions/28935159/sqoop-import-says-can-find-class-tablename
            # http://ingest.tips/2015/02/06/use-sqoop-transfer-csv-data-local-filesystem-relational-database/

            $ hdfs dfs -mkdir -p /tmp/sqoop-hqlgree2/
            $ hdfs dfs -put /tmp/sqoop-hqlgree2/* /tmp/sqoop-hqlgree2/
            $ hdfs dfs -ls /tmp/sqoop-hqlgree2/compile

            $ hdfs dfs -rm -r /tmp/sqoop-hqlgree2/compile
            $ hdfs dfs -rm -r /user/hqlgree2/sqoopdata/employee
            
            $ sqoop import -libjars /tmp/sqoop-hqlgree2/compile/199c329d20fd26512ff0f9593facdb3e/employee.jar \
            --connect jdbc:mysql://localhost/sqoop --username sqoop --password sqoop --table employee \
            --target-dir sqoopdata/employee

    1. --target-dir

            absolute path => /usr/hqlgree2/sqoopdata/employee
            relative path => sqoopdata/employee

            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee \
            --target-dir /usr/hqlgree2/sqoopdata/employee

            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee \
            --target-dir sqoopdata/employee

    1. --warehouse-dir

            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee \
            --warehouse-dir sqoopdata -m 1

    1. -m integer

            default is 4
            will generate 4 files on hdfs
            part-m-00000 ... part-m-00003

            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee \
            --target-dir sqoopdata/employee -m 1

    1. --where "filter"

            --where "pk between 2 and 4"
    
    1. --check-column pkcol

    1. --incremental append

    1. --incremental last_modified

    1. --last-value integer

            # 1. list database `sqoop` table `employee` data
            $ mysql -u root sqoop -e "select * from employee;"
            +--------+----------+-----------+
            | emp_id | emp_name | emp_title |
            +--------+----------+-----------+
            |      1 | name1    | dev       |
            |      2 | name2    | pm        |
            |      3 | name3    | qa        |
            |      4 | name4    | test      |
            |      5 | name5    | dev       |
            +--------+----------+-----------+

            # 2. import
            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee \
            --warehouse-dir sqoopdata -m 1

            $ hdfs dfs -ls /user/hqlgree2/sqoopdata/employee/
            Found 2 items
            -rw-r--r--   1 hqlgree2 supergroup          0 2015-05-27 22:02 /user/hqlgree2/sqoopdata/employee/_SUCCESS
            -rw-r--r--   1 hqlgree2 supergroup         59 2015-05-27 22:02 /user/hqlgree2/sqoopdata/employee/part-m-00000

            # 3. insert new data into table `employee`
            $ mysql -u root sqoop -e "insert into employee values (6, 'name6', 'dev'), (7, 'name7', 'test'), (8, 'name8', 'dev');"

            # 4. list database `sqoop` table `employee` data
            $ mysql -u root sqoop -e "select * from employee;"
            +--------+----------+-----------+
            | emp_id | emp_name | emp_title |
            +--------+----------+-----------+
            |      1 | name1    | dev       |
            |      2 | name2    | pm        |
            |      3 | name3    | qa        |
            |      4 | name4    | test      |
            |      5 | name5    | dev       |
            |      6 | name6    | dev       |
            |      7 | name7    | test      |
            |      8 | name8    | dev       |
            +--------+----------+-----------+

            # 5. import `6-8` row into hdfs
            $ sqoop import --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee \
            --warehouse-dir sqoopdata -m 1 \
            --check-column emp_id \
            --incremental append \
            --last-value 5

            $ hdfs dfs -ls /user/hqlgree2/sqoopdata/employee/
            Found 3 items
            -rw-r--r--   1 hqlgree2 supergroup          0 2015-05-27 22:02 /user/hqlgree2/sqoopdata/employee/_SUCCESS
            -rw-r--r--   1 hqlgree2 supergroup         59 2015-05-27 22:02 /user/hqlgree2/sqoopdata/employee/part-m-00000
            -rw-r--r--   1 hqlgree2 supergroup         37 2015-05-27 22:04 /user/hqlgree2/sqoopdata/employee/part-m-00001
            
            $ hdfs dfs -cat /user/hqlgree2/sqoopdata/employee/part-m-00000
            1,name1,dev
            2,name2,pm
            3,name3,qa
            4,name4,test
            5,name5,dev
            
            $ hdfs dfs -cat /user/hqlgree2/sqoopdata/employee/part-m-00001
            6,name6,dev
            7,name7,test
            8,name8,dev

    1. --options-file filename

            $ pico mysql.options
            import
            --connect
            jdbc:mysql://localhost:3306/sqoop
            --username
            sqoop
            --password
            sqoop

            $ sqoop --options-file mysql.options --table employee -m 1

1. import data to `hive`

    1. multi-command (all null value data)

            $ sqoop --options-file mysql.options --table employee \
            --warehouse-dir sqoopdata -m 1

            $ hive
            hive> show tables;
            hive> desc emps;

            $ sqoop create-hive-table --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --password sqoop --table employee --hive-table emps

            $ hive
            hive> select * from emps;
            # no data
            hive> load data inpath '/usr/hqlgree2/sqoopdata/employee/part-m-00000' into table emps;
            hive> select * from emps;
            # all null value data
            hive> drop table emps;
            # will also delete `part-m-00000` on hdfs

    1. multi-command (correct data)

            $ sqoop --options-file mysql.options --table employee \
            --warehouse-dir sqoopdata -m 1

            $ hive
            hive> show tables;
            hive> desc emps;

            $ sqoop create-hive-table --connect jdbc:mysql://localhost/sqoop \
            --username sqoop --table employee --hive-table emps \
            --mysql-delimiters

            $ hive
            hive> select * from emps;
            # no data
            hive> load data inpath '/usr/hqlgree2/sqoopdata/employee/part-m-00000' into table emps;
            hive> select * from emps;
            # all null value data
            hive> drop table emps;
            # will also delete `part-m-00000` on hdfs

    1. create-hive-table

    1. hive-import

            # equals multi-command (correct data)
            $ sqoop --options-file mysql.options --table employee \
            --hive-table emps --hive-import

### export

1. --export-dir

            $ sqoop export --connect jdbc:mysql://localhost/test \
            --username sqoop -P --table student -m 1 \
            --export-dir '/usr/hqlgree2/sqoopdata/part-m-00000'

1. --options-file

            $ pico export.txt
            export
            --connect
            jdbc:mysql://localhost:3306/test
            --username
            sqoop

            $ sqoop --options-file export.txt -P --table student -m 1 \
            --export-dir '/usr/hqlgree2/sqoopdata/part-m-00000'

1. --update-key

            $ sqoop --options-file export.txt --table student \
            --export-dir '/usr/hqlgree2/sqoopdata/student_detail.txt' \
            --update-key stu_id

### eval list-databases list-tables

1. list-databases

            $ sqoop list-databases --connect jdbc:mysql://localhost \
            --username sqoop

1. list-tables

            $ sqoop list-tables    --connect jdbc:mysql://localhost/mysql \
            --username sqoop

1. eval

            $ sqoop eval --connect jdbc:mysql://localhost/mysql \
            --username sqoop --query "select * from user;"

### sqlserver

1. config

    1. [download and config sqljdbc](http://hortonworks.com/hadoop-tutorial/import-microsoft-sql-server-hortonworks-sandbox-using-sqoop/)

            $ curl -L 'http://download.microsoft.com/download/0/2/A/02AAE597-3865-456C-AE7F-613F99F850A8/sqljdbc_4.0.2206.100_enu.tar.gz' | tar xz
            $ cd sqljdbc_4.0/enu
            $ cp sqljdbc4.jar /usr/local/Cellar/sqoop/1.4.5/libexec/lib/
            $ hdfs dfs -put sqljdbc4.jar /usr/local/Cellar/sqoop/1.4.5/libexec/lib/

    1. codegen and put to hdfs

            $ sqoop codegen --connect 'jdbc:sqlserver://192.168.100.100;database=sqoop;username=sa;password=as' --table employee
            ...
            INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-hqlgree2/compile/38c5c07622a71bc6695d397b2ad36e40/employee.jar

            $ hdfs dfs -mkdir /tmp/sqoop-hqlgree2/compile/38c5c07622a71bc6695d397b2ad36e40/
            
            $ hdfs dfs -put /tmp/sqoop-hqlgree2/compile/38c5c07622a71bc6695d397b2ad36e40/* \
            /tmp/sqoop-hqlgree2/compile/38c5c07622a71bc6695d397b2ad36e40/

            $ hdfs dfs -rm -r /user/hqlgree2/mssql/employee

    1. options-file

            $ pico mssql.options 
            import
            -libjars
            /tmp/sqoop-hqlgree2/compile/38c5c07622a71bc6695d397b2ad36e40/employee.jar
            --connect
            'jdbc:sqlserver://192.168.100.100;database=sqoop;username=sa;password=sa'

1. list-databases

            $ sqoop list-databases --connect jdbc:sqlserver://192.168.100.100:1433 \
            --username sa --password as

1. import

            $ sqoop --options-file mssql.options --table employee --target-dir mssql/employee
            $ hdfs dfs -rm -r /user/hqlgree2/mssql/employee

            $ sqoop import --connect jdbc:sqlserver://192.168.100.100:1433;database=demo \
            --username sa --password as --hive-import -m 1 \
            --columns "Name, Year, Rating"

            # if table has no primary key
            # 1. use `--split-by`
            # 2. sequential import with `-m 1`

1. export

            $ sqoop --options-file mssql.options --table employee \
            --export-dir /user/hqlgree2/sqoopdata/employee/part-m-00000 \
            --import-fields-terminated-by ','

            $ sqoop export --connect jdbc:sqlserver://192.168.100.100:1433;database=demo \
            --username sa --password as --table movie \
            --export-dir /user/hqlgree2/sqoopdata/employee/part-m-00000 \
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