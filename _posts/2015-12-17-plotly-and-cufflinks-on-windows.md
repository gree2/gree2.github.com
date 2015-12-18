---
layout: post
title: "plotly and cufflinks on windows"
description: ""
category: [python]
tags: [pip, plotly, cufflinks, windows, vc++ 9.0]
---
{% include JB/setup %}


### install

1. install `plotly`

			> pip install plotly --upgrade

1. install `cufflinks`

			> pip install cufflinks --upgrade

### fixed

1. error: 

	1. Microsoft Visual C++ 9.0 is required (Unable to find vcvarsall.bat). Get it from [http://aka.ms/vcpython27](https://www.microsoft.com/en-us/download/confirmation.aspx?id=44266)

			Command "C:\Anaconda\python.exe -c "import setuptools, tokenize;__file__='c:\\us
			ers\\admini~1\\appdata\\local\\temp\\pip-build-adjg4q\\numpy\\setup.py';exec(com
			pile(getattr(tokenize, 'open', open)(__file__).read().replace('\r\n', '\n'), __f
			ile__, 'exec'))" install --record c:\users\admini~1\appdata\local\temp\pip-0ap7x
			o-record\install-record.txt --single-version-externally-managed --compile" faile
			d with error code 1 in c:\users\admini~1\appdata\local\temp\pip-build-adjg4q\num
			py

	1. after download and install

			> pip install cufflinks --upgrade
