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

            # $ docker run --name gredis -d redis
            $ docker run -p 6379:6379 --name gredis -d redis

1. redis instance info

    1. check

            $ docker ps -a
            CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
            2cb741aea9e2        redis               "docker-entrypoint.sh"   28 minutes ago      Up 28 minutes       6379/tcp            gredis

    1. ip

            $ docker inspect --format='\{\{.NetworkSettings.IPAddress\}\}' gredis
            172.17.0.3

1. python code

    1. [zset.poc.py on my github](https://github.com/gree2/hobby/blob/master/python/p.redis/zset.poc.py)

            #! coding: utf-8

            import redis
            import time
            from datetime import datetime, timedelta

            DAYS = 10
            # $ docker run -p 6379:6379 --name gredis -d redis
            conn = redis.Redis(host='localhost', port=6379, db=1)


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


            def zset_poc():
                """zset_poc"""
                # now = datetime.now() - timedelta(days=DAYS)
                now = datetime.strptime('20160514112233', '%Y%m%d%H%M%S') - timedelta(days=DAYS)
                days = [now - timedelta(days=-day) for day in range(DAYS)]
                times = [time.mktime(day.timetuple()) for day in days]
                datas = [(date_format(key), value) for key, value in zip(days, times)]

                zset_key = 'day_ts'
                # remove old test if any
                cnt = conn.zcard(zset_key)
                if cnt > 0:
                    print 'remove', cnt, 'in zset'
                    conn.zremrangebyrank(zset_key, 0, cnt)
                # field 5 to 9 or -5 to -1
                (key_start, ts_start) = datas[-5]
                (key_stop, ts_stop) = datas[-1]

                # 0 20160504 1462370790.0
                # 1 20160505 1462457190.0
                # 2 20160506 1462543590.0
                # 3 20160507 1462629990.0
                # 4 20160508 1462716390.0
                # 5 20160509 1462802790.0 <=
                # 6 20160510 1462889190.0
                # 7 20160511 1462975590.0
                # 8 20160512 1463061990.0
                # 9 20160513 1463148390.0 <=
                for i, (key, value) in enumerate(datas):
                    print i, key, value,
                    if key == key_start:
                        print '<=',
                    if key == key_stop:
                        print '<=',
                    print
                    conn.zadd(zset_key, key, value)
                # ('20160513', 1463148092.0) ('20160509', 1462802492.0)
                print datas[-5], datas[-1]
                # ['20160509', '20160510', '20160511', '20160512', '20160513']
                print conn.zrange(zset_key, 5, 9)
                print conn.zrange(zset_key, -5, -1)
                print conn.zrangebyscore(zset_key, ts_start, ts_stop)

            if __name__ == '__main__':
                zset_poc()


1. cleanup

    1. stop

            $ docker stop gredis

    1. remove

            $ docker rm gredis
