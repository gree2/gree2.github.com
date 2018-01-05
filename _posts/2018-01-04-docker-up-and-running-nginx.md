---
layout: post
title: "docker up and running nginx"
description: ""
category: [docker]
tags: [docker, nginx]
---
{% include JB/setup %}

### docker image

1. hosting static content

	1. use cli

			docker run --name gnginx -v /some/content:/usr/share/nginx/html:ro -d nginx

	1. use dockerfile

			$ nano Dockerfile
			FROM nginx
			COPY static-html-directory /usr/share/nginx/html

			$ docker build -t some-content-nginx .
			$ docker run --name some-nginx -d some-content-nginx

	1. exposing external port

			$ docker run --name gnginx -d -p 8080:80 some-content-nginx

1. config

	1. use config file

			$ docker -run --name gnginx -d -v /host/path/nginx.conf:/etc/nginx/nginx.conf:ro -p 8080:80 some-content-nginx

	1. use default config file

			$ docker run --name tmp-nginx -d -p 8888:80 nginx
			$ docker cp tmp-nginx:/etc/nginx/nginx.conf /host/path/nginx.conf
			$ docker rm -f tmp-nginx

### reference

1. [nginx.org](http://nginx.org/)

1. [docker nginx](https://store.docker.com/images/nginx)