---
layout: post
title: "database adjust memory usage in sql server"
description: ""
category: [database]
tags: [memory, usage, mssql]
---
{% include JB/setup %}

### summery

1. by default mssql dynamically grows and shrink the size of its buffer pool (cache)

1. as long as sufficient memory `(4MB to 10MB)` is available to prevent paging, mssql buffer pool will continue to grow

1. this behavior alone `does not` indicate a memory leak, this behavior is typical and is an `intended behavior` of the mssql buffer pool

1. you can establish upper and lower limits for how much memory (buffer pool) is used by mssql database engine with the
    
    * min server memory
    * max server memory

1. notice that the `max server memory` option only limits the size of mssql buffer pool

1. `max server memory` option `does not` limit a remaining unreserved memory area that mssql leaves for allocations of other components such as

    * extended stored procedures
    * com object
    * non-shared dlls
    * exes
    * mapi components

### set max server memory use sql

    sp_configure 'show advanced options', 1;
    go
    reconfigure;
    go
    sp_configure 'max server memory', 4096;
    go
    reconfigure;
    go

### virtual memory manager

* 32-bit os provide access to 4GB of virtual address space

    * 2GB of vm is private per process and available for app use
    * 2GB is reserved for os use
    * all os editions include a switch that can provide app with access up to 3GB ov vm, limiting the os to 1GB
    * when the 32-bit mssql running on 64-bit os its user available vms is the full 4GB

> PAE is automatically enabled on if the server is using hot-add memory devices. in this case, you do not have to use the /PAE switch on a system that is configured to use hot-add memory devices. in all other case, you must use the /PAE switch in the Boot.ini file to take advantage of memory over 4GB

1. 4GB address space is mapped to the available physical memory by the windows virtual memory manager (vmm)

    * the physical memory accessible by awe therefore depends on which os you are using

1. virtural address systems allow the over-commitment of physical memory

    * so that the ratio of virtual to physical memory can exceed 1:1
    * as a result larger programs can run on computers with a variety of physical memory configuratons

1. the `min server memory` and `max server memory` options are advanced options

    * if you are using the `sp_configure` system stored procedures to change theses settings
    * you can change them only when `show advanced options` is set to `1`
    * these settings `take effect immediately without a server restart`

### running multiple instances of mssql

when you are running multiple instances of the database engine, there are three approaches you can use to manage memory

1. use `max server memory`

1. use `min server memory`

1. do nothing (not recommended)

### references

1. [msdn](https://msdn.microsoft.com/en-us/library/ms178067(v=sql.105).aspx)

1. [support microsoft](http://support.microsoft.com/en-us/kb/321363)