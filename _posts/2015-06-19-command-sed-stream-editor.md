---
layout: post
title: "command sed stream editor"
description: ""
category: [command]
tags: [sed, stream, editor]
---
{% include JB/setup %}


### info

1. `sed` short for `stream editor`

1. `filter` and `transform` text

### [advanced demo](http://www.thegeekstuff.com/2009/10/unix-sed-tutorial-advanced-sed-substitution-examples/)

1. sed substitution delimiter

    1. `@` delimiter

            $ sed 's@/path/to/file@/path/toto/file@g' path.txt

    1. `/` delimiter

            $ sed 's/\/path\/to\/file/\/path\/toto\/file/g' path.txt

1. sed `&` get matched string

    1. substitute `usr/bin` to `usr/bin/local`

            $ sed 's@/usr/bin@&local@g' path.txt
            # & in the replacement part
            # will be replace with `/usr/bin`

    1. match the whole line

            # & replaces whatever matches with give regexp
            $ sed 's@^.*$@<<<&>>>@g' path.txt
            # `^.*$` matches the whole line

1. grouping and back-references

    1. get only the first path in each line

            $ sed 's/\(\/[^:]*\).*/\1/g' path.txt
            # `\(\/[^:]*\)` matches the path
            # avaiale before first `:` comes
            # `\1` replaces the first matched group

    1. multigrouping

            $ sed '$s@\([^:]*\):\([^:]*\):\([^:]*\)@\3:\2:\1@g' path.txt

    1. get the list of usernames in `/etc/passwd` file

            $ sed 's/\([^:]*\).*/\1/' /etc/passwd

    1. parenthesize first character of each word

            $ echo "Welcome To The Geek Stuff" | sed 's/\(\b[A-Z]\)/\(1\)/g'

    1. commify the simple numbers

            $ cat numbers
            1234
            12121
            3434
            123

            $ sed 's/\(^\|[^0-9.]\)\([0-9]\+\)\([0-9]\{3\}\)/\1\2,\3/g' numbers