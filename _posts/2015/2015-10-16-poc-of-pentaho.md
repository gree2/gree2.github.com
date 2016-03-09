---
layout: post
title: "poc of pentaho"
description: ""
category: [bi]
tags: [poc, pentaho, prd]
---
{% include JB/setup %}


### poc of prd

1. datasource

    1. jdbc

            # h2
            # hadoop hive
            # hadoop hive 2
            # impala
            # native mondrian
            # pentaho data services
            # sap erp system
            # teradata

            # db2
            # mysql
            # ms sql server
            # ms sql server - native
            # oracle
            # postgresql
            # sqlite
            # sybase

    1. metadata

    1. mongodb

    1. pentaho data integration

    1. olap

            # 1. pentaho analysis
            # 2. pentaho analysis - denormalized
            # 3. olap4j
            # 4. olap4j - denormalized

    1. olap - advanced

            # 1. pentaho analysis - custom
            # 1. pentaho analysis - custom denormalized

    1. xml

    1. table

    1. advanced

            # 1. scriptable
            # 2. external
            # 3. sequence generator
            # 4. openerp data access

    1. community data access

1. groups

    1. group header

            # 1. display  -> label -> attributes.field

    1. structure -> master report -> group

            # 1. attributes.group -> [field_name] <- data group by this field
            # 2. attributes.name  -> groupname

    1. functions and page breaks

            # structure -> master report -> group
            # style.pagebreak-after = true

    1. edit group

            # 1. name
            name of the group

            # 2. selected fields
            used as grouping criteria           

1. parameters

    1. value type

            # 1. String
            # 2. Boolean
            # 3. Number
            # 4. Date
            # 5. Date - sql
            # 6. Time
            # 7. Timestamp
            # 8. Double
            # 9. Float
            # 10. Integer
            # 11. Long
            # 12. Short
            # 13. Byte
            # 14. BigInteger
            # 15. BigDecimal
            # 16. TableModel
            # 17. Object

    1. display type

            # 1. drop down
            # 2. single value list
            # 3. multi value list
            # 4. radio button
            # 5. check box
            # 6. single selection button
            # 7. multi selection button
            # 8. text box
            # 9. text area
            # 10. date picker

    1. demo

            Name
            Label
            Value Type
            Mandatory
            Display Type
            Query
            Value
            Display Name

    1. demo nested parameters

            # SelectYear
            # SelectMonth

            # SelectMonth depends on SelectYear

1. formulas

    1. prd impl open standard `OpenFormula`

            OpenFormula is an open format for exchanging recalculated formulas between office application implementations, particularly for spreadsheets. OpenFormula defines the types, syntax, and semantics for calculated formulas, including many predefined functions and operations, so that formulas can be exchanged between applications and produce substantively equal outputs when recalculated with equal inputs. Both closed and open source software can implement OpenFormula.

    1. 2 ways to create

            # 1. Common -> Open Formula
            # 2. Style/Attribs tab press `add` button

    1. demo calc

            # Functions -> Add Functions -> Common -> Open Formula
            =[sum_amount]*[count_rental]

            # Formula can be drag and drop as fields
            # or set by Number-field's attributes.field

    1. demo styles `style.bg-color` with formulas

            # 1. use default value 4 and 6
            =IF([sum_amount]<4;"#990000";IF([sum_amount]<6;"#cccc00";"#66cc00"))

            # 2. use parameters
            =IF([sum_amount]<[para_threshold_lower];"#990000";IF([sum_amount]<[para_threshold_upper];"#cccc00";"#66cc00"))

1. charts

    1. types

        # 1. bar chart
        # 2. line chart
        # 3. area chart
        # 4. pie chart
        # 5. multi pie chart
        # 6. bar line chart
        # 7. ring chart
        # 8. bubble chart
        # 9. scatter plot chart
        # 10. xy bar chart
        # 11. xy line chart
        # 12. xy bar line chart
        # 13. extended xy line chart
        # 14. waterfall chart
        # 15. radar chart
        # 16. xy area line chart
        # 17. thermometer chart

1. subreports

1. publish and run reports

1. hyperlinks and sparklines

    1. hyperlinks

            # 1. two parts
            link
            target

            # 2. types of hyperlinks in prd
            self           -> pass value to parameters of the current report
            url            -> access external resource
            pentaho repo   -> link reports xactions from pentaho repo
            manual linking -> config hyperlink manually and navigate using html anchors

    1. sparklines

1. env variables style sheets crosstabs

1. embedded in web app

### poc of bi server