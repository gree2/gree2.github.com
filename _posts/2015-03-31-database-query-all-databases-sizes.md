---
layout: post
title: "database query all databases sizes"
description: ""
category: [database]
tags: [query]
---
{% include JB/setup %}

### sql

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

### references

* [stackoverflow](http://stackoverflow.com/questions/5945360/sql-server-2008-how-to-query-all-databases-sizes)