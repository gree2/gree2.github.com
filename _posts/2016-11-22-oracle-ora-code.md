---
layout: post
title: "oracle ora code"
description: ""
category: [database]
tags: [oracle, mac, sqlplus, tnsping, tns, ora]
---
{% include JB/setup %}


### ora code

1. ora-21561

    1. message: oid generation failed

    1. on macos sierra

    1. fixed

            $ hostname
            gree2

            $ cat /etc/hosts
            127.0.0.1 localhost gree2

            # these 2 must be match

### tns test

1. tnsnames.ora

    1. file D:\app\Administrator\product\11.2.0\client_1\NETWORK\ADMIN\tnsnames.ora

            ORCL =
            (DESCRIPTION =
                (ADDRESS_LIST =
                    (ADDRESS = (PROTOCOL = TCP)(HOST = 127.0.0.1)(PORT = 1521))
                )
                (CONNECT_DATA =
                    (SERVICE_NAME = ORCL)
                )
            )

1. sqlplus

    1. test conn

            $ sqlplus /nolog
            > conn scott/tigger as sysdba

1. tnsping

    1. test conn

            $ tnsping orcl
