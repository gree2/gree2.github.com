---
layout: post
title: "batch net use xcopy and mkdir"
description: ""
category: [batch]
tags: [net, xcopy, mkdir, date, time]
---
{% include JB/setup %}

### net use

1. create folder name from `date` and `time`

        set folder_name=%date:~0,4%%date:~5,2%%date:~8,2%%time:~0,2%%time:~3,2%

1. mount `remote folder` to `Z:` drive

        net use Z: \\192.168.1.2\D$\folder password /user:username /persistent:no

1. create a folder on remote computer

        set new_folder=Z:\%folder_name%
        mkdir %new_folder%

1. copy `local folder` to `remote new folder`

        xcopy local_folder %new_folder%\local_folder\ /i /s /e /h /q

1. unmounted drive `Z:`

        net use * /delete /y

1. all together

        set folder_name=%date:~0,4%%date:~5,2%%date:~8,2%%time:~0,2%%time:~3,2%
        net use Z: \\192.168.1.2\D$\folder password /user:username /persistent:no
        set new_folder=Z:\%folder_name%
        mkdir %new_folder%
        xcopy local_folder %new_folder%\local_folder\ /i /s /e /h /q
        net use * /delete /y

### date time string

1. date and time output

    * get date

            > date /t
            20150323
            ^^^^^^^^
            ||||||||
            01234567

    * get time

            > time /t
            17:37
            ^^^^^
            |||||
            01234

1. substring

    * get year form date
    
            %date:~0,4%
            2015

    * get month form date

            %date:~5,2%
            03

    * get day from date

            %date:~8,2%
            23
    * get hour form time

            %time:~0,2%
            17

    * get minute from time

            %time:~3,2%
            37

1. concat them all

        set folder_name=%date:~0,4%%date:~5,2%%date:~8,2%%time:~0,2%%time:~3,2%