---
layout: post
title: "relocate docker default location"
description: ""
category: [docker]
tags: [devops, tar, mv systemctl]
---
{% include JB/setup %}


### steps

1. stop docker

        $ systemctrl stop docker

2. [custom docker daemon options](https://docs.docker.com/engine/admin/systemd/#custom-docker-daemon-options)

        $ nano /etc/docker/daemon.json

        {
            "graph": "/home/lib/docker",
            "storage-driver": "overlay"
        }

3. backup /var/lib/docker

        $ tar -zcC /home/lib/var.lib.docker-$(date +%Y%m%d%H%M%S).tar.gz /var/lib/docker
        $ tar czvf var.lib.docker-$(date +%Y%m%d%H%M%S).tar.gz /var/lib/docker

4. move to new location

        $ mv /var/lib/docker /home/lib/

5. start docker

        $ systemctrl stop docker
