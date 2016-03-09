---
layout: post
title: "mac diskutil command"
description: ""
category: [mac, command]
tags: [erase, partition, split, merge]
---
{% include JB/setup %}


### info

1. native apple command line utility

    1. manipulating disks, partitions and raid set

    1. include magnetic hard disks, cds/dvds and flash drives

    1. most options except `list` require root access

1. the default apple partitioning scheme uses the last physical partition on a disk for storing data

### demo

1. [from commandlinemac](http://commandlinemac.blogspot.com/2008/12/using-diskutil.html)

    1. info on all available disks and their partitions

            $ diskutil list

    1. more detailed info on a particular disk or partitions

            $ diskutil info disk-or-partition

    1. check fs structure of a partition

            $ diskutil verifyVolume partition

    1. if errors are found fix them with

            $ diskutil repairVolume partition

    1. finding out about raid sets

            $ diskutil listRAID

1. [from langui](https://langui.sh/2011/04/02/using-dd-in-os-x/) using dd in os x

    1. find out device labels

            $ $ diskutil list
            /dev/disk0
               #:                       TYPE NAME                    SIZE       IDENTIFIER
               0:      GUID_partition_scheme                        *251.0 GB   disk0
               1:                        EFI EFI                     209.7 MB   disk0s1
               2:          Apple_CoreStorage                         250.1 GB   disk0s2
               3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
            /dev/disk1
               #:                       TYPE NAME                    SIZE       IDENTIFIER
               0:                  Apple_HFS ssd                    *249.8 GB   disk1
                                             Logical Volume on disk0s2
                                             8AE3A13B-FBF6-4000-84F9-A2586C9B65BA
                                             Unencrypted
            /dev/disk2
               #:                       TYPE NAME                    SIZE       IDENTIFIER
               0:     FDisk_partition_scheme                        *16.0 GB    disk2
               1:             Windows_FAT_32 GREE2                   16.0 GB    disk2s1

    1. to write to a mounted usb flash drive

            # we need to unmount it
            # but ejecting it in the normal mac way won't work here
            $ diskutil unmountDisk /dev/disk2

    1. write image file

            $ dd if=inputfile.img of/dev/disk2

1. [from theinstructional part 1](http://www.theinstructional.com/guides/disk-management-from-the-command-line-part-1)

    1. erasing disks

            $ diskutil eraseDisk JHFS+ GREE2 /dev/disk2

    1. filesystem

            # JHFS+ mac os extended(Journaled)
            $ diskutil listFilesystems
            Formattable file systems

            These file system personalities can be used for erasing and partitioning.
            When specifying a personality as a parameter to a verb, case is not considered.
            Certain common aliases (also case-insensitive) are listed below as well.

            -------------------------------------------------------------------------------
            PERSONALITY                     USER VISIBLE NAME                               
            -------------------------------------------------------------------------------
            ExFAT                           ExFAT                                           
            Free Space                      Free Space                                      
              (or) free
            MS-DOS                          MS-DOS (FAT)                                    
            MS-DOS FAT12                    MS-DOS (FAT12)                                  
            MS-DOS FAT16                    MS-DOS (FAT16)                                  
            MS-DOS FAT32                    MS-DOS (FAT32)                                  
              (or) fat32
            HFS+                            Mac OS Extended                                 
            Case-sensitive HFS+             Mac OS Extended (Case-sensitive)                
              (or) hfsx
            Case-sensitive Journaled HFS+   Mac OS Extended (Case-sensitive, Journaled)     
              (or) jhfsx
            Journaled HFS+                  Mac OS Extended (Journaled)                     
              (or) jhfs+

    1. erasing volumes

            # erasing volumes doesn't erase
            # or affect the entire disk
            # only the volume you specify

            # external drive
            # partitioned with two volumes
            # FirstVolume and SecondVolume
            # to erase SecondVolume
            $ diskutil eraseVolume JHFS+ New /Volumes/SecondVolume

    1. reformatting volumes

            $ diskutil reformat /Volumes/SecondVolume

    1. renaming volumes

            $ diskutil rename "{current name of volume}" "{new name}"

    1. verifying and repairing

            # verifying and repairing volumes
            $ diskutil verifyVolume /Volumes/SecondVolume \
            diskutil repairVolume /Volumes/SecondVolume

            # verifying disk permissions
            $ diskutil verifyPermissions /Volumes/SecondVolume \
            diskutil repair Permissions /Volumes/SecondVolume

1. [from theinstructional part 2](http://www.theinstructional.com/guides/disk-management-from-the-command-line-part-2)

    1. partition a disk

            $ diskutil partitionDisk /dev/disk2 GPT JHFS+ New 0b
            # GPT guid partition table
            # APM Apple partition map
            # MBR Master boot records

            # powerpc-based mac or windows pc boot from GPT
            # intel macs can only boot from guid partitions

            # `0b` represents the size of the partition to create
            # b bytes, m megabytes, g gigabytes
            # `0b` use all available free space

    1. multiple partitioning

            $ diskutil partitionDisk /dev/disk2 GPT JHFS+ First 10g \
            JHFS+ Second 10g \
            JHFS+ Third 10g \
            JHFS+ Fourth 0b

    1. splitting partitions

            $ diskutil splitPartition /dev/disk2s6 JHFS+ Test1 10g JHFS+ Test2 0b

    1. merging partitions

            $ diskutil mergePartitions JHFS+ NewName disk2s4 disk2s6