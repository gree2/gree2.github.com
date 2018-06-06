---
layout: post
title: "docker save and load"
description: ""
category: [docker]
tags: [save, load, pv]
---
{% include JB/setup %}


### usages

1. export and import image

    1. save

            $ docker save --help                                             ✱

            Usage:  docker save [OPTIONS] IMAGE [IMAGE...]

            Save one or more images to a tar archive (streamed to STDOUT by default)

            Options:
              -o, --output string   Write to a file, instead of STDOUT

    1. load

            $ docker load --help                                             ✱

            Usage:  docker load [OPTIONS]

            Load an image from a tar archive or STDIN

            Options:
              -i, --input string   Read from tar archive file, instead of STDIN
              -q, --quiet          Suppress the load output

1. export and import container

    1. export

            $ docker export --help                                           ✱

            Usage:  docker export [OPTIONS] CONTAINER

            Export a container's filesystem as a tar archive

            Options:
              -o, --output string   Write to a file, instead of STDOUT



    1. import

            $ docker import --help                                           ✱

            Usage:  docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]

            Import the contents from a tarball to create a filesystem image

            Options:
              -c, --change list      Apply Dockerfile instruction to the created image
              -m, --message string   Set commit message for imported image

### demo

1. [how to copy docker images from one host to another without via repository](https://stackoverflow.com/questions/23935141/how-to-copy-docker-images-from-one-host-to-another-without-via-repository)

    1. install [epel](https://fedoraproject.org/wiki/EPEL)

            # RHEL/CentOS 6
            $ yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm

            # RHEL/CentOS 7
            $ yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
            # enable the optional & extras repos
            $ subscription-manager repos --enable "rhel-*-optional-rpms" --enable "rhel-*-extras-rpms"

    1. install pv

            $ yum install pv

    1. demo

            $ docker save centos:7.3.1611 | bzip2 | pv | ssh root@dc 'bunzip2 | docker load'
            $ docker save centos:7.2.1511 | bzip2 | pv | ssh root@dc 'bunzip2 | docker load'
