---
layout: post
title: "command file and iconv read and convert file encoding"
description: ""
category: [command]
tags: [file, iconv, mime, encoding, convert]
---
{% include JB/setup %}


### solution

1. find out character encoding of a file

            $ file --mime-encoding filename
            filename: ISO-8859-1

1. check this kind of text encoding is supported

            $ iconv -l | grep ISO-8859-1

1. convert `ISO-8859-1` to `UTF-8`

            $ iconv -f ISO-8859-1 -t UTF-8 filename > filenewname