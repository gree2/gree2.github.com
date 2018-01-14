---
layout: post
title: "ansible your docker container"
description: ""
category: [auto, docker]
tags: [auto, docker, playbook, st3, docker_container_module, command, sublime-build, pip, hosts, playbook]
---
{% include JB/setup %}


### steps

1. run shell from st3

    1. create sublime-build file

            $ nano bash.sublime-build
            {
                "cmd"       : ["$file"],
                "selector"  : "source.shell",
                "shell"     : "bash"
            }

1. install ansible

    1. use pip

            $ pip install -U ansible

    1. setup hosts file

            $ nano /usr/local/etc/ansible/hosts
            [docker]
            dc

1. create playbook yml

    1. use mysql image

            $ nano deploy.yml
            ---
            - hosts: docker
              remote_user: root
              become: yes
              gather_facts: no
              vars:
                container_name: "forestry"
                container_image: "mysql:5.5.58"
                working_dir: "/root/deploy/docker/mysql/forestry"

              tasks:
                - name: remove container
                  docker_container:
                    name: "{{ container_name }}"
                    state: absent

                - name: create container
                  docker_container:
                    name: "{{ container_name }}"
                    image: "{{ container_image }}"
                    volumes:
                      - "{{ working_dir }}/conf:/etc/mysql/"
                      - "{{ working_dir }}/data:/var/lib/mysql"
                    ports:
                      - "33061:3306"
                    env:
                      MYSQL_ROOT_PASSWORD: 123456
                    command: mysqld --character-set-server=gbk

1. run playbook

    1. use deploy.sh

            $ nano deploy.sh
            ansible-playbook deploy.yml

    1. in st3

            press command+b

### references

1. [ansible docker_container_module](http://docs.ansible.com/ansible/latest/docker_container_module.html#docker-container)

1. [use custom command to launch mysql container with right collation](http://blog.it-premium.com.ua/tags/mysql/)
