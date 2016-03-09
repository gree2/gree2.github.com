---
layout: post
title: "database useful queries"
description: ""
category: [database]
tags: [query]
---
{% include JB/setup %}

### sql

1. database query all databases sizes

    * [stackoverflow](http://stackoverflow.com/questions/5945360/sql-server-2008-how-to-query-all-databases-sizes)

            with fs
            as
            (
                select database_id, type, size * 8.0 / 1024 / 1024 size
                from sys.master_files
            )
            select 
                name,
                (select sum(size) from fs where type = 0 and fs.database_id = db.database_id) DataFileSizeMB,
                (select sum(size) from fs where type = 1 and fs.database_id = db.database_id) LogFileSizeMB
            from sys.databases db order by DataFileSizeMB

1. find all tables containing column with specified name

    * [stackoverflow](http://stackoverflow.com/questions/4849652/find-all-tables-containing-column-with-specified-name)

            select  
                    s.[name]            'Schema',
                    t.[name]            'Table',
                    c.[name]            'Column',
                    d.[name]            'Data Type',
                    d.[max_length]      'Max Length',
                    d.[precision]       'Precision',
                    c.[is_identity]     'Is Id',
                    c.[is_nullable]     'Is Nullable',
                    c.[is_computed]     'Is Computed',
                    d.[is_user_defined] 'Is UserDefined',
                    t.[modify_date]     'Date Modified',
                    t.[create_date]     'Date created'
            from        sys.schemas s
            inner join  sys.tables  t
            on s.schema_id = t.schema_id
            inner join  sys.columns c
            on t.object_id = c.object_id
            inner join  sys.types   d
            on c.user_type_id = d.user_type_id

    * [blog.sqlauthority.com](http://blog.sqlauthority.com/2008/08/06/sql-server-query-to-find-column-from-all-tables-of-database/)

            SELECT t.name AS table_name,
            SCHEMA_NAME(schema_id) AS schema_name,
            c.name AS column_name
            FROM sys.tables AS t
            INNER JOIN sys.columns c ON t.OBJECT_ID = c.OBJECT_ID
            WHERE c.name LIKE '%ColumnName%'
            ORDER BY schema_name, table_name;

1. get all tables all column info

    * query

            SELECT
            (case when a.colorder=1 then d.name else '-' end) N'TableName',
            a.colorder N'ColumnOrder',
            a.name N'ColumnName',
            (case when COLUMNPROPERTY( a.id,a.name,'IsIdentity')=1 then '1' else '0' end) N'Identity',
            (case when (SELECT count(*)
            FROM sysobjects
            WHERE (name in (SELECT name FROM sysindexes
                WHERE (id = a.id) AND (indid in (SELECT indid FROM sysindexkeys
                    WHERE (id = a.id) AND (colid in (SELECT colid FROM syscolumns
                        WHERE (id = a.id) AND (name = a.name)
                    ))
                ))
            )) AND (xtype = 'PK'))>0 then '1' else '0' end) N'PrimaryKey',
            b.name N'DataType',
            a.length N'Length',
            COLUMNPROPERTY(a.id,a.name,'PRECISION') as N'Precision',
            isnull(COLUMNPROPERTY(a.id,a.name,'Scale'),0) as N'Scale',
            (case when a.isnullable=1 then '1' else '0' end) N'Nullable',
            isnull(e.text,'') N'Default',
            isnull(g.[value],'') AS N'Description'
            FROM syscolumns a left join systypes b
            on a.xtype=b.xusertype
            inner join sysobjects d
            on a.id=d.id and d.xtype='U' and d.name<>'dtproperties'
            left join syscomments e
            on a.cdefault=e.id

            --For SQL2005
            --left join sysproperties g
            --on a.id=g.id AND a.colid = g.smallid

            --For SQL2008
            left join sys.extended_properties g
            on a.id=g.major_id AND a.colid = g.major_id 
            order by object_name(a.id), a.colorder

    * results (= is not in the query result)

TableName=|=ColumnOrder=|=ColumnName=|=Identity=|=PrimaryKey=|=DataType=|=Length=|=Precision=|=Scale=|=Nullable=|=Default=|=Description
MSreplication=options=|=1=|=optname=|=0=|=0=|=nvarchar=|=256=|=128=|=0=|=0=|=|=
-=|=2=|=value=|=0=|=0=|=bit=|=1=|=1=|=0=|=0=|=|=
-=|=3=|=major=version=|=0=|=0=|=int=|=4=|=10=|=0=|=0=|=|=
-=|=4=|=minor=version=|=0=|=0=|=int=|=4=|=10=|=0=|=0=|=|=
-=|=5=|=revision=|=0=|=0=|=int=|=4=|=10=|=0=|=0=|=|=
-=|=6=|=install=failures=|=0=|=0=|=int=|=4=|=10=|=0=|=0=|=|=  

1. batch export database tables to csv files

    1. without header [reference ms sql server 2008 how to export all the tables into csv](http://serverfault.com/questions/210526/ms-sql-server-2008-how-to-export-all-the-tables-into-csv)

            SELECT 'bcp '
            + QUOTENAME(DB_NAME())
            + '.'
            + QUOTENAME(SCHEMA_NAME(schema_id))
            + '.'
            + QUOTENAME(name)
            + ' '+'out'+' '
            + name + '.csv -w -t"," -E -S '
            + @@servername
            + ' -T'
            FROM sys.objects
            WHERE TYPE='u' AND is_ms_shipped=0
            order by name

            file mime encoding is: utf-16le

    1. [with utf-8 format](http://www.saianudeep.com/post/31881768923/export-csv-from-sql-server)

            bcp mydb.mytable out c:\data.csv -c -T -C65001 -r^^ -t^_

            -C65001 is to use utf-8 format

            -r is used to specify the row delimiter

            -t is used to specify the field separator

            '^^'=ctrl+shift+6
            '^_'=ctrl+shift+-

            octal representation
            '^^'='\036'
            '^_'='\037'

1. batch export database tables to csv files

    1. with header [reference export sql data into csv file with header using sqlcmd](http://www.dbascript.com/export-sql-data-into-csv-file-with-header-using-sqlcmd)

            select 'EXEC ' + QUOTENAME(DB_NAME()) + '..xp_cmdshell ''sqlcmd -E -s"|" -W -Q "set nocount on; set ansi_warnings off; SELECT * FROM '
            + QUOTENAME(DB_NAME()) + '.dbo.'
            + QUOTENAME(name)
            + '" | findstr /V /C:"-" /B > '
            + name+'.csv'''
            FROM sys.objects
            WHERE TYPE='u' AND is_ms_shipped=0
            order by name

            file mime encoding is: iso-8859-1

    1. with header and utf-8 encoding

            select 'EXEC ' + QUOTENAME(DB_NAME()) + '..xp_cmdshell ''sqlcmd -E -s"|" -W -f 65001 -w 4000 -Q "set nocount on; set ansi_warnings off; SELECT * FROM '
            + QUOTENAME(DB_NAME()) + '.dbo.'
            + QUOTENAME(name)
            + '" -o "'
            + name+'.csv"'''
            FROM sys.objects
            WHERE TYPE='u' AND is_ms_shipped=0
            order by name

            -f 65001 is utf-8 encoding

1. configure to use xp_cmdshell

    * [reference enable xp cmdshell sql server](http://stackoverflow.com/questions/5131491/enable-xp-cmdshell-sql-server?answertab=votes#tab-top)

            -- To allow advanced options to be changed.
            EXEC sp_configure 'show advanced options', 1
            GO
            -- To update the currently configured value for advanced options.
            RECONFIGURE
            GO
            -- To enable the feature.
            EXEC sp_configure 'xp_cmdshell', 1
            GO
            -- To update the currently configured value for this feature.
            RECONFIGURE
            GO

### generate backup script for all databases

1. [use cursor](http://www.mssqltips.com/sqlservertip/1070/simple-script-to-backup-all-sql-server-databases/)

    * sql

            DECLARE @sd_name VARCHAR(50)   -- database name  
            DECLARE @path_bak VARCHAR(256)     -- path for backup files  
            DECLARE @fileName VARCHAR(256) -- filename for backup  

            -- specify database backup directory
            SET @path_bak = 'D:\\database\\150507\\'

            -- specify filename format
            -- SELECT @fileDate = CONVERT(VARCHAR(20), GETDATE(), 112) 

            DECLARE db_cursor CURSOR FOR  
            SELECT name FROM master.dbo.sysdatabases 
            WHERE name NOT IN ('master', 'model', 'msdb', 'tempdb', 'ReportServer', 'ReportServerTempDB')

            OPEN db_cursor   
            FETCH NEXT FROM db_cursor INTO @sd_name

            WHILE @@FETCH_STATUS = 0   
            BEGIN   
                SET @fileName = @path_bak + '\\' + @sd_name + '.bak'
                --BACKUP DATABASE @sd_name TO DISK = @fileName  

                print '--' + @sd_name
                print 'BACKUP DATABASE [' + @sd_name + ']'
                print 'TO DISK = N''' + @fileName + ''''
                print 'WITH NOFORMAT, NOINIT,'
                print 'NAME = N''' + @sd_name + ''','
                print 'SKIP, NOREWIND, NOUNLOAD,  STATS = 10'
                print 'GO'
                print ''
             
                FETCH NEXT FROM db_cursor INTO @sd_name
            END   

            CLOSE db_cursor
            DEALLOCATE db_cursor

    * output

            -- mydb1
            BACKUP DATABASE [mydb1]
            TO  DISK = N'D:\\database\\20150507\\mydb1.bak'
            WITH NOFORMAT, NOINIT,
            NAME = N'mydb1',
            SKIP, NOREWIND, NOUNLOAD,  STATS = 10
            GO
             
            -- mydb2
            BACKUP DATABASE [mydb2]
            TO  DISK = N'D:\\database\\20150507\\mydb2.bak'
            WITH NOFORMAT, NOINIT,
            NAME = N'mydb2',
            SKIP, NOREWIND, NOUNLOAD,  STATS = 10
            GO

            ...

1. [use a single sql](http://weblogs.asp.net/jongalloway/461034)

    * sql

            SELECT 'BACKUP DATABASE ' + QUOTENAME(CATALOG_NAME)
                + ' TO DISK = N''D:\\database\\150507\\' + CATALOG_NAME + '.bak'''
                + ' WITH NOFORMAT, NOINIT,'
                + ' NAME = N''' + CATALOG_NAME + ''','
                + ' SKIP, NOREWIND, NOUNLOAD,  STATS = 10'
            FROM INFORMATION_SCHEMA.SCHEMATA
            WHERE CATALOG_NAME NOT IN ('master','tempdb','msdb','model','Northwind','pubs')

    * output

            BACKUP DATABASE [mydb1] TO DISK = N'D:\\database\\150507\\mydb1.bak' WITH NOFORMAT, NOINIT, NAME = N'mydb1', SKIP, NOREWIND, NOUNLOAD,  STATS = 10
            BACKUP DATABASE [mydb2] TO DISK = N'D:\\database\\150507\\mydb2.bak' WITH NOFORMAT, NOINIT, NAME = N'mydb2', SKIP, NOREWIND, NOUNLOAD,  STATS = 10
            ...


### generate restore script for all databases

1. [use cursor](http://sqldavel.blogspot.com/2013/03/script-to-generate-restore-statements.html)

    * sql

            -- desc: Generate 'restore database' statements
            -- help: Edit 'where' clause and/or backup path information in variables as needed
            -- Run script to generate sql, then edit/modify as needed and run selected sql statements.
            --
            set nocount ON

            DECLARE @RestoreDB VARCHAR (8000)
            DECLARE @Move varchar (8000)
            DECLARE @MoveWithoutComma varchar (8000) 
            DECLARE @Go varchar (8000) 
            DECLARE @firsttime varchar (5) -- = 'True'
            DECLARE @sd_name varchar(255)
            DECLARE @smf_name varchar(255)
            DECLARE @smf_physical_name varchar (255)
            DECLARE @hold_sd_name varchar (255) 
            DECLARE @path_bak VARCHAR(256)     -- path for backup files  

            select @firsttime = 'True'
            -- specify database backup directory
            SET @path_bak = 'D:\\database\\150507\\'

            DECLARE Database_cursor CURSOR FOR 
            SELECT sd.name,smf.name, smf.physical_name
            FROM sys.master_files AS smf inner join sys.databases AS sd
            on smf.database_id = sd.database_id
            -- Edit where clause as desired
            where sd.name not in ('master', 'tempdb', 'msdb', 'model')
            ORDER BY sd.name

            OPEN Database_cursor
            FETCH NEXT FROM Database_cursor INTO @sd_name, @smf_name, @smf_physical_name

            Select @hold_sd_name = @sd_name
            WHILE @@FETCH_STATUS = 0 
            Begin
                if @firsttime = 'True'
                    Begin
                        -- Edit backup path as needed
                        print '-- ' + @sd_name
                        select @RestoreDB = 'RESTORE DATABASE [' + @sd_name + ']' + char(13) + char(10)
                        + 'FROM DISK = ''' + @path_bak
                        + @sd_name + '.bak''' + char(13) + char(10) + 'WITH MOVE ''' + @smf_name + ''' TO '''
                        + @smf_physical_name + ''', replace ,' + 'stats = 10' + char(13) + char(10)
                        print @RestoreDB
                 
                        select @Go = 'GO' + char(13) + char(10)

                        FETCH NEXT FROM Database_cursor INTO @sd_name, @smf_name, @smf_physical_name

                        if @hold_sd_name <> @sd_name or @@FETCH_STATUS <> 0
                            select @hold_sd_name = @sd_name
                        else
                            select @firsttime = 'False'
                    end
                else
                    Begin
                        -- Move with comma ','
                        select @Move = '     MOVE ''' + @smf_name + ''' TO '''
                        + @smf_physical_name + ''','
                        -- Last 'Move' without comma ','
                        select @MoveWithoutComma = '     MOVE ''' + @smf_name + ''' TO '''
                        + @smf_physical_name + ''''
                        FETCH NEXT FROM Database_cursor INTO @sd_name, @smf_name, @smf_physical_name
                        if @hold_sd_name <> @sd_name or @@FETCH_STATUS <> 0
                            Begin
                                print @MoveWithoutComma
                                print @Go
                                select @hold_sd_name = @sd_name
                                select @firsttime = 'True'
                            end
                        else
                            print @Move
                    end
            end

            CLOSE Database_cursor
            DEALLOCATE Database_cursor
            GO

    * output

            -- mydb1
            RESTORE DATABASE [mydb1]
            FROM DISK = 'D:\\database\\150507\\mydb1.bak'
            WITH MOVE 'mydb1' TO 'D:\Program Files\Microsoft SQL Server\MSSQL10.MSSQLSERVER\MSSQL\DATA\mydb1.mdf', replace ,stats = 10
                 MOVE 'mydb1_log' TO 'D:\Program Files\Microsoft SQL Server\MSSQL10.MSSQLSERVER\MSSQL\DATA\mydb1_1.LDF'
            GO
            -- mydb2
            RESTORE DATABASE [mydb2]
            FROM DISK = 'D:\\database\\150507\\mydb2.bak'
            WITH MOVE 'mydb2' TO 'D:\Program Files\Microsoft SQL Server\MSSQL10.MSSQLSERVER\MSSQL\DATA\mydb2.mdf', replace ,stats = 10
                 MOVE 'mydb2_log' TO 'D:\Program Files\Microsoft SQL Server\MSSQL10.MSSQLSERVER\MSSQL\DATA\mydb2_1.LDF'

### datetime

1. convert unix timestamp to weekday

    1. steps

            timestamp => date => weekday

    1. [how can i convert bigint unix timestamp to datetime in sql server](http://stackoverflow.com/questions/2904256/how-can-i-convert-bigint-unix-timestamp-to-datetime-in-sql-server)


            select dateadd(s, 1274756183, '19700101 05:00:00:000')


    1. [datename method in sqlserver](http://stackoverflow.com/questions/1431496/datename-method-in-sqlserver)

            select datename(dw, getdate())

    1. in all

            select day_name, count(*) as day_count from (
                select datename(dw,
                    convert(varchar(50), dateadd(s, cast(time_stamp as int), '19700101 05:00:00:000'), 121)
                    ) as day_name
                from ratings
            ) as day_group group by day_name

### alter

1. table

    1. [sql server alter rename table](http://stackoverflow.com/questions/5335528/sql-server-alter-rename-table)

            -- to rename a column:
            sp_rename 'table_name.old_column_name', 'new_column_name' , 'column';

            -- to rename a table:
            sp_rename 'old_table_name','new_table _name';

    1. batch rename all tables with prefix

            --sp_rename old_table_name, new_table_name
            SELECT 'sp_rename ' + t.name + ', prefix' + t.name AS sql,
            SCHEMA_NAME(schema_id) AS schema_name,
            c.name AS column_name
            FROM sys.tables AS t
            INNER JOIN sys.columns c ON t.OBJECT_ID = c.OBJECT_ID
            ORDER BY schema_name, sql;

1. column

    1. batch add column

            SELECT 'ALTER TABLE ' + t.name + ' ADD BMDM VARCHAR(20) NULL;' AS sql,
            SCHEMA_NAME(schema_id) AS schema_name,
            c.name AS column_name
            FROM sys.tables AS t
            INNER JOIN sys.columns c ON t.OBJECT_ID = c.OBJECT_ID
            WHERE c.name LIKE '%part_of_column_name%'
            ORDER BY schema_name, sql;

### querying the sql server system catalog faq

[msdn](https://msdn.microsoft.com/en-us/library/ms345522.aspx)