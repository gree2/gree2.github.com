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

### querying the sql server system catalog faq

[msdn](https://msdn.microsoft.com/en-us/library/ms345522.aspx)