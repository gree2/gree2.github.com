---
layout: post
title: "tool ssh keys"
description: ""
category: [tool]
tags: [ssh, keys, rsa, linux, mac, windows]
---
{% include JB/setup %}

### generating an ssh key

1. generating

            $ ssh-keygen -t rsa -C "hqlgree2@gmail.com"

1. show your public key

            $ cat ~/.ssh/id_rsa.pub

### copy the complete key

1. windows

            > clip < id_rsa.pub

1. linux

            $ xclip -sel clip < id_rsa.pub

1. mac

            $ pbcopy < id_rsa.pub

### [reference](https://gitlab.com/help/ssh/README)