---
layout: post
title: "docker up and running kong"
description: ""
category: [docker]
tags: [docker, kong]
---
{% include JB/setup %}


### docs

1. how to

    1. link kong to cassandra or postgresql container

            $ docker run -d --name kong-database -p 9042:9042 cassandra:3

            $ docker run -d --name kong-database -p 5432:5432 -e "POSTGRES_USER=kong" -e "POSTGRES_DB=kong" postgres:9.4

    1. prepare datatase

            $ docker run --rm \
            --link kong-database:kong-database \
            -e "KONG_DATABASE=postgres" \
            -e "KONG_PG_HOST=kong-database" \
            -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
            kong kong migrations up

            # both cassandra and postgresql are configured
            # but KONG_DATABASE can only choose 1 from

    1. start kong

            $ docker run -d --name kong \
            --link kong-database:kong-database \
            -e "KONG_DATABASE=postgres" \
            -e "KONG_PG_HOST=kong-database" \
            -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
            -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
            -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
            -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
            -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
            -p 8000:8000 \
            -p 8443:8443 \
            -p 8001:8001 \
            -p 8444:8444 \
            kong:latest

    1. use kong

            $ curl -i http://localhost:8001/

### references

1. [https://store.docker.com/images/kong](https://store.docker.com/images/kong)

1. [https://store.docker.com/images/postgres](https://store.docker.com/images/postgres)
