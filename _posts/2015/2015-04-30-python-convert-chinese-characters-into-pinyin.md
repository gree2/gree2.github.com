---
layout: post
title: "python convert chinese characters into pinyin"
description: ""
category: [python]
tags: [chinese, character, pinyin, cjklib, hanzi2pinyin]
---
{% include JB/setup %}


### packages

1. [cjklib](https://code.google.com/p/cjklib/)

1. [pinyin](https://pypi.python.org/pypi/pinyin/0.1.2)

1. [hanzi2pinyin](https://pypi.python.org/pypi/hanzi2pinyin)

1. [Unidecode](https://pypi.python.org/pypi/Unidecode/0.04.1)

* [uc-to-py.tbl](http://www.ic.unicamp.br/~stolfi/voynich/Notes/061/uc-to-py.tbl)

### package `cjklib`

1. check my [post anaconda on mac]({% post_url 2015-04-03-anaconda-on-mac %})

1. we can not use cjklib on osx-64 now

    * `No packages found` in current `osx-64 channels` matching: `cjklib`

1. code snippet from [www.codeitive.com](http://www.codeitive.com/0yHgVkPgVU/python-convert-chinese-characters-into-pinyin-with-cjklib.html)

            from cjklib.characterlookup import CharacterLookup

            source_file = 'input.txt'
            dest_file = 'output.txt'

            s = open(source_file, 'r')
            d = open(dest_file, 'w')

            cjk = CharacterLookup('T')

            for line in s:
                p = line.split('\t')
                for p_shard in p:
                    for c in p_shard:
                        readings = cjk.getReadingForCharacter(c.encode('utf-8'), 'Pinyin')
                        d.write(readings[0].encode('utf-8'))
                    d.write('\t')
                d.write('\n')

            s.close()
            d.close()

### package `pinyin`

1. install form pypi [source]((https://pypi.python.org/pypi/pinyin/0.1.2))

            $ cd pinyin-0.1.2
            $ python setup.py install

1. code snippet

            >>> import pinyin
            >>> pinyin.get_pinyin(u'你好')
            'nihao'
            >>> pinyin.get_initial(u'你好')
            'n h'

### package `hanzi2pinyin`

1. we can not use cjklib on osx-64 now

    * `No packages found` in current `osx-64 channels` matching: `cjklib`

1. install form pypi [source](https://pypi.python.org/pypi/hanzi2pinyin)

1. code snippet

            >>> from hanzi2pinyin import hanzi2pinyin
            >>> hanzi2pinyin(u"你好")
            ... nihao

### package `Unidecode`

1. install from pypi [source](https://pypi.python.org/pypi/Unidecode/0.04.1)

1. code snippet

            from unidecode import unidecode
            print unidecode(u"\u5317\u4EB0")

            # That prints: Bei Jing