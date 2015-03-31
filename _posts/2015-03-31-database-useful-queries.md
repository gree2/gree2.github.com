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


### querying the sql server system catalog faq

[msdn](https://msdn.microsoft.com/en-us/library/ms345522.aspx)