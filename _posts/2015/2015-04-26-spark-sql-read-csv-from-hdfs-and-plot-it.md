---
layout: post
title: "spark sql read csv from hdfs and plot it"
description: ""
category: [bigdata]
tags: [spark, sql, csv, plot]
---
{% include JB/setup %}


### code

            from pyspark.sql import SQLContext
            from pyspark.sql.types import StructField, IntegerType, StringType, StructType
            %matplotlib inline
            import matplotlib.pyplot as plt
            import matplotlib
            print matplotlib.matplotlib_fname()
            matplotlib.rcParams['font.sans-serif'] = ['Microsoft YaHei']
            matplotlib.rcParams['axes.unicode_minus'] = False

            csv_dt = sc.textFile("hdfs://localhost:9000/python/temp.csv", use_unicode=False)
            header = csv_dt.take(2)
            for h in header: print h

            footer = csv_dt.filter(lambda x: x not in header)
            for f in footer.collect(): print f

            parts = footer.map(lambda x: x.split("|"))
            for p in parts.collect(): print p

            table = parts.map(lambda x: (int(x[0]), x[1].decode('utf-8')))
            table

            for t in table.collect():
                c, n = t
                print c, n

            f_code = StructField("code", IntegerType(), True)
            f_name = StructField("name", StringType(), True)
            sqlCtx = SQLContext(sc)
            schema = StructType([f_code, f_name])
            df = sqlCtx.createDataFrame(table, schema)
            df.registerTempTable("temp_table")

            results = df.select(df.code, df.name).orderBy(df.code)
            resultsPandas = results.toPandas()
            resultsPandas

            #xlabels = df.select(df.name).map(lambda x: x.name).collect()
            #for l in xlabels: print l

            fig = resultsPandas.plot(kind="bar", x="name")
            #fig.set_xticklabels(xlabels, rotation=45)
            #fig

            plt.clf()
