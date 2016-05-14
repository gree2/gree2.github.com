---
layout: post
title: "python with docker redis"
description: ""
category: [python]
tags: [python, docker, redis]
---
{% include JB/setup %}


### setup

1. create a container with docker

    1. pull redis image

            $ docker pull redis

    1. start a redis instance

            $ docker run --name gredis -d redis

1. redis instance info

    1. check

            $ docker ps -a
            CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
            2cb741aea9e2        redis               "docker-entrypoint.sh"   28 minutes ago      Up 28 minutes       6379/tcp            gredis

    1. ip

            $ docker inspect --format='{{.NetworkSettings.IPAddress}}' gredis
            172.17.0.3

1. python code

            #! coding: utf-8

            import redis
            import time
            from datetime import datetime, timedelta

            conn = redis.Redis(host='172.17.0.3', port=6379, db=1)


            def redis_info():
                """redis_info"""
                info = conn.info()
                for key in info:
                    print "%s: %s" % (key, info[key])

                print 'dbsize: %s' % conn.dbsize()
                print "ping %s" % conn.ping()


            def date_format(date):
                """date_format"""
                return date.strftime("%Y%m%d")

            def zset_day_ts():
                """zset_day_ts"""
                now = datetime.now()
                days = [now - timedelta(days=day) for day in range(40)]
                times = [time.mktime(day.timetuple()) for day in days]
                datas = [(date_format(key), value) for key, value in zip(days, times)]
                print datas
                # [('20160514', 1463218683.0),
                #  ('20160513', 1463132283.0),
                #  ('20160512', 1463045883.0),
                #  ('20160511', 1462959483.0),
                #  ('20160510', 1462873083.0),
                #  ('20160509', 1462786683.0),
                #  ('20160508', 1462700283.0),
                #  ('20160507', 1462613883.0),
                #  ('20160506', 1462527483.0),
                #  ('20160505', 1462441083.0),
                #  ('20160504', 1462354683.0),
                #  ('20160503', 1462268283.0),
                #  ('20160502', 1462181883.0),
                #  ('20160501', 1462095483.0),
                #  ('20160430', 1462009083.0),
                #  ('20160429', 1461922683.0),
                #  ('20160428', 1461836283.0),
                #  ('20160427', 1461749883.0),
                #  ('20160426', 1461663483.0),
                #  ('20160425', 1461577083.0),
                #  ('20160424', 1461490683.0),
                #  ('20160423', 1461404283.0),
                #  ('20160422', 1461317883.0),
                #  ('20160421', 1461231483.0),
                #  ('20160420', 1461145083.0),
                #  ('20160419', 1461058683.0),
                #  ('20160418', 1460972283.0),
                #  ('20160417', 1460885883.0),
                #  ('20160416', 1460799483.0),
                #  ('20160415', 1460713083.0),
                #  ('20160414', 1460626683.0),
                #  ('20160413', 1460540283.0),
                #  ('20160412', 1460453883.0),
                #  ('20160411', 1460367483.0),
                #  ('20160410', 1460281083.0), <= 5
                #  ('20160409', 1460194683.0),
                #  ('20160408', 1460108283.0),
                #  ('20160407', 1460021883.0),
                #  ('20160406', 1459935483.0),
                #  ('20160405', 1459849083.0)] <= 0
                for (key, value) in datas:
                    conn.zadd('day_ts', key, value)

            # zset_day_ts()
            print conn.zrange('day_ts', 0, 5)
            # ['20160405', '20160406', '20160407', '20160408', '20160409']
            print conn.zrangebyscore('day_ts', 1459849083.0, 1460281083.0)
            # ['20160405', '20160406', '20160407', '20160408', '20160409']

1. cleanup

    1. stop

            $ docker stop gredis

    1. remove

            $ docker rm gredis
