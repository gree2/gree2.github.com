---
layout: post
title: "websocket"
description: ""
category: [web, python]
tags: [websocket, nodejs, node, python, websocket-client]
---
{% include JB/setup %}


### hello world demo

1. server side

    1. install `ws` library

            $ npm install ws

    1. nodejs code `server.js`

            var WebSocketServer = require('ws').Server,
                wss = new WebSocketServer({port: 8181});

            wss.on('connection', function(ws) {
                console.log('client connected');
                ws.on('message', function(message) {
                    console.log(message);
                    ws.send(message)
                });
                ws.onclose = function(e) {
                    console.log(e.code + "" + e.reason);
                }
            })

    1. run the server

            $ node server.js

1. client side

    1. install python [websocket-client](https://github.com/liris/websocket-client)

            $ pip install websocket-client

    1. client code `client.py`

            #! coding: utf-8

            from websocket import create_connection


            def send_msg(msg):
                """send_msg"""
                ws = create_connection("ws://localhost:8181/")
                print "client> {}".format(msg)
                ws.send(msg)
                print "server< {}".format(ws.recv())
                ws.close()

            send_msg('hello world')
