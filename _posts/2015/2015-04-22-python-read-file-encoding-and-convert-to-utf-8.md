---
layout: post
title: "python read file encoding and convert to utf-8"
description: ""
category: [python]
tags: [encoding, convert, libmagic]
---
{% include JB/setup %}


### install package

1. install [libmagic](http://www.brambraakman.com/blog/comments/installing_libmagic_in_mac_os_x_for_python-magic/)

			$ brew install libmagic

1. install [python-magic](https://pypi.python.org/pypi/python-magic)

			$ python setup.py install

### code read file encoding

* [reference determine the encoding of text file](http://stackoverflow.com/questions/436220/python-is-there-a-way-to-determine-the-encoding-of-text-file)

			import magic

			blob = open('file.csv').read()
			m = magic.Magic(mime_encoding=True)
			encoding = m.from_buffer(blob)
			print(encoding)

### code convert file encoding

* [reference how to convert a file to utf-8 in python](http://stackoverflow.com/questions/191359/how-to-convert-a-file-to-utf-8-in-python)

			source = open('file.csv')
			target = open('file_utf8.csv', "w")

			target.write(unicode(source.read(), encoding).encode("utf-8"))

	* but after convert this file cannot `cat`