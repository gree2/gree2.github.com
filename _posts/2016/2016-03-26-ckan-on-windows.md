---
layout: post
title: "ckan on windows"
description: ""
category: [setup]
tags: [ckan, windows, postgresql]
---
{% include JB/setup %}


### steps

1. install python environment

    1. use [anaconda](https://docs.continuum.io/)

1. setup a [postgresql](http://www.postgresql.org/) test database

    1. [download page](http://www.enterprisedb.com/products-services-training/pgdownload#windows)

    1. run `postgresql-9.5.1-1-windows-x64.exe`

            # 1. welcome to the postgresql setup wizard
            # 2. installation directory `C:\Program Files\PostgreSQL\9.5`
            # 3. data directory `C:\Program Files\PostgreSQL\9.5\data`
            # 4. password 123456
            # 5. port 5432
            # 6. locale [default locale]
            # 7. ready to install
            # 8. installing

    1. run `pgadmin3.exe` in C:\Program Files\PostgreSQL\9.5\bin

            # 1. click `postgresql 9.5 (localhost：5432)`
            # 2. input `password`
            # 3. right click `login rules` select `new login role`
            # 4. role name `ckanuser` set password, default privileges click ok

            # 5. right click `databases` select `new database...`
            # 6. name `ckan` and owner `ckanuser` encoding utf-8 click ok

    1. install [psycopg](http://www.stickpeople.com/projects/python/win-psycopg/2.6.1/psycopg2-2.6.1.win-amd64-py2.7-pg9.4.4-release.exe)

1. env and source

    1. virtualenv

            >  conda create -n ckan2.5.2env
            Error: too few arguments, must supply command line package specs or --file

            You can specify one or more default packages to install when creating
            an environment.  Doing so allows you to call conda create without
            explicitly providing any package names.

            To set the provided packages, call conda config like this:

                conda config --add create_default_packages PACKAGE_NAME

            >  conda create -n ckan2.5.2env ipython
            Fetching package metadata: ....
            Solving package specifications: ...........
            Package plan for installation in environment C:\Anaconda\envs\ckan2.5.2env:

            The following packages will be downloaded:

                package                    |            build
                ---------------------------|-----------------
                vs2008_runtime-9.00.30729.1|                0         1.1 MB  defaults
                python-2.7.11              |                4        23.1 MB  defaults
                decorator-4.0.9            |           py27_0          12 KB  defaults
                setuptools-20.3            |           py27_0         760 KB  defaults
                wheel-0.29.0               |           py27_0         121 KB  defaults
                pip-8.1.1                  |           py27_0         1.5 MB  defaults
                traitlets-4.2.1            |           py27_0         108 KB  defaults
                ipython-4.1.2              |           py27_1         982 KB  defaults
                ------------------------------------------------------------
                                                       Total:        27.8 MB

            The following NEW packages will be INSTALLED:

                decorator:        4.0.9-py27_0   defaults
                ipython:          4.1.2-py27_1   defaults
                ipython_genutils: 0.1.0-py27_0   defaults
                path.py:          8.1.2-py27_1   defaults
                pickleshare:      0.5-py27_0     defaults
                pip:              8.1.1-py27_0   defaults
                pyreadline:       2.1-py27_0     defaults
                python:           2.7.11-4       defaults
                setuptools:       20.3-py27_0    defaults
                simplegeneric:    0.8.1-py27_0   defaults
                traitlets:        4.2.1-py27_0   defaults
                vs2008_runtime:   9.00.30729.1-0 defaults
                wheel:            0.29.0-py27_0  defaults

            Proceed ([y]/n)? y

            Fetching packages ...
            vs2008_runtime 100% |###############################| Time: 0:00:03 391.51 kB/s
            python-2.7.11- 100% |###############################| Time: 0:00:39 615.83 kB/s
            decorator-4.0. 100% |###############################| Time: 0:00:00  45.51 kB/s
            setuptools-20. 100% |###############################| Time: 0:00:02 286.69 kB/s
            wheel-0.29.0-p 100% |###############################| Time: 0:00:01  92.55 kB/s
            pip-8.1.1-py27 100% |###############################| Time: 0:00:05 273.88 kB/s
            traitlets-4.2. 100% |###############################| Time: 0:00:01  69.10 kB/s
            ipython-4.1.2- 100% |###############################| Time: 0:00:02 415.38 kB/s
            Extracting packages ...
            [      COMPLETE      ]|##################################################| 100%
            Linking packages ...
            [      COMPLETE      ]|##################################################| 100%
            #
            # To activate this environment, use:
            # > activate ckan2.5.2env
            #

    1. git clone

            > git clone https://github.com/ckan/ckan.git
            > git branch -a
            ...
            remotes/origin/release-v2.5.2
            ...

    1. checkout branch

            > git checkout -b release-v2.5.2
            > ls
            bin/           CONDUCT.rst           Dockerfile                 README.rst        test.ini
            CHANGELOG.rst  contrib/              LICENSE.txt                requirements.in   test-core.ini
            circle.yml     CONTRIBUTING.rst      MANIFEST.in                requirements.txt  who.ini
            ckan/          dev-requirements.txt  pip-requirements-docs.txt  setup.cfg
            ckanext/       doc/                  profile_tests.py           setup.py

    1. install requirements to virtual env

            # active your virtual environment
            > activate ckan2.5.2env
            Activating environment "C:\Anaconda\envs\ckan2.5.2env"...

            # install packages to virtual env
            [ckan2.5]> pip install -r requirements.txt

    1. install from source

            # develop
            # [ckan2.5]> python setup.py develop

            # install
            [ckan2.5]> python setup.py install

1. config

### reference

1. [how-to-install-ckan-1.8-on-windows](https://github.com/ckan/ckan/wiki/How-to-Install-CKAN-1.8-on-Windows)