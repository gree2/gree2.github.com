---
layout: post
title: "pip updating packages"
description: ""
category: [python]
tags: [pip, updating, package, grep, cut, xargs, subprocess, call]
---
{% include JB/setup %}


### solution

1. command

    1. list all pkgs

            $ pip freeze --local
            alabaster==0.7.6
            alembic==0.8.7
            amqp==1.4.9
            anaconda-client==1.2.1
            anyjson==0.3.3
            argcomplete==1.0.0
            astropy==1.0.5
            Babel==2.1.1
            backports-abc==0.4
            backports.ssl-match-hostname==3.4.0.2
            ...
            -e git+git@gitlab.com:username/pkg.git@b351b0910698eb67e8bb31e6c1a4b8a454968cbd#egg=pkg
            ...

    1. remove pkg definitions

            $ pip freeze --local | grep -v '^\-e'
            # this will remove lines start with `-e`

    1. get pkg name from field

            $ pip freeze --local | grep -v '^\-e' | cut -d = -f 1

    1. add `-n1` to xargs prevents stopping everything if updating one pkg fails

            $ pip freeze --local | grep -v '^\-e' | cut -d = -f 1  | xargs -n1 pip install -U

1. code

        import pip
        from subprocess import call

        for dist in pip.get_installed_distributions():
            call("pip install --upgrade " + dist.project_name, shell=True)

### reference

1. [upgrading-all-packages-with-pip](http://stackoverflow.com/questions/2720014/upgrading-all-packages-with-pip)
