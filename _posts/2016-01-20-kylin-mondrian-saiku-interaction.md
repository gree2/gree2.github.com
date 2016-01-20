---
layout: post
title: "kylin mondrian saiku interaction"
description: ""
category: [bi]
tags: [kylin, mondrian, saiku, jdbc, mdx, cube]
---
{% include JB/setup %}


### steps

1. download software

1. hive create table and load demo data

1. kylin sync table create cube and build

1. saiku deploy create schema and add data source

1. test query form saiku against kylin

### step 1

1. download [commons-httpclient-3.1.jar](https://github.com/gree2/kylin-mondrian-interaction/blob/master/saiku/commons-httpclient-3.1.jar)

1. download [kylin-jdbc-1.0-incubating.jar](https://github.com/gree2/kylin-mondrian-interaction/blob/master/kylin/kylin-jdbc-1.0-incubating.jar)

1. download [mondrian-4.4-lagunitas-SNAPSHOT-with-kylin-dialect.jar](https://github.com/gree2/kylin-mondrian-interaction/tree/master/mondrian)

1. download [saiku-saiku-3.7.4.zip](https://github.com/OSBI/saiku/releases)

### step 2 hive create table and load demo data

1. prepare data

    1. unzip `saiku-saiku-3.7.4.zip`

    1. find `saiku-server/data/foodmart_h2.sql`

    1. edit this sql and load into mysql

    1. export data from mysql to csv files

1. create table

    1. email me for `foodmart.create.hive.tables.sql` this file

1. load data into tables

    1. email me for `foodmart.load.data.into.table.sql` file and `csv` fiels

### step 3 kylin sync table create cube and build

1. sync table

    1. login into kylin webui

    1. click `tables` tab

    1. choose project(create one if you don't have one)

    1. click `load hive table` button and input these tablenames

            account, agg_c_10_sales_fact_1997, agg_c_14_sales_fact_1997, agg_c_special_sales_fact_1997, agg_g_ms_pcat_sales_fact_1997, agg_l_03_sales_fact_1997, agg_l_04_sales_fact_1997, agg_l_05_sales_fact_1997, agg_lc_06_sales_fact_1997, agg_lc_100_sales_fact_1997, agg_ll_01_sales_fact_1997, agg_pl_01_sales_fact_1997, category, currency, customer, days, department, employee, employee_closure, expense_fact, inventory_fact_1997, inventory_fact_1998, position, product, product_class, promotion, region, reserve_employee, salary, sales_fact_1997, sales_fact_1998, sales_fact_dec_1998, store, store_ragged, time_by_day, warehouse, warehouse_class

    1. click `sync` button

1. create cube

    1. login into kylin webui

    1. click `cubes` tab

    1. click `+cube` button

    1. and in `cube designer` there are 8 steps

            # 1. cubeinfo
            # 2. data model
            # 3. dimensions
            # 4. measures
            # 5. filter
            # 6. refresh setting
            # 7. advanced setting
            # 8. overview

    1. click `save`

1. build cube

    1. after you click `save` you will see a cubes table

    1. in the `actions` column of you cube, select `build`

    1. click `jobs` tab

    1. from `jobs` tab you will see your cube's build status

    1. when done (progress column will show 100%) click `query` tab

1. query against your cube

    1. click `query` tab

    1. select your project

    1. input your query

            # something like this

            SELECT
                COFFEE_BAR,
                STORE_TYPE,
                STORE_ID,
                COUNT(*),
                SUM(STORE_SQFT),
                SUM(GROCERY_SQFT)
            FROM STORE
            GROUP BY
                COFFEE_BAR,
                STORE_TYPE,
                STORE_ID

### step 4 saiku deploy create schema and add data source

1. deploy saiku

    1. unzip it to somewhere

    1. put these jar files to `saiku-server/tomcat/webapps/saiku/WEB-INF/lib`

            commons-httpclient-3.1.jar
            kylin-jdbc-1.0-incubating.jar
            mondrian-4.4-lagunitas-SNAPSHOT-with-kylin-dialect.jar

    1. delete these old jar files

            commons-httpclient-20020423.jar
            mondrian-4.3.0.1-SPARK.jar

    1. from `saiku-server` folder start saiku

            start-saiku.sh
            or
            start-saiku.bat

1. create schema, please check my post [2015-11-08-saiku-design-a-schema]({% post_url 2015-11-08-saiku-design-a-schema %})

    1. create a schema(xml file)

            <?xml version='1.0'?>
            <Schema metamodelVersion="4.0" name="FoodMartKylin">
                <PhysicalSchema>
                    <Table name="STORE">
                        <Key>
                            <Column name="STORE_ID"/>
                        </Key>
                    </Table>
                </PhysicalSchema>
                <Dimension key="Store Id" name="Store" table="STORE">
                    <Attributes>
                        <Attribute hasHierarchy="false" name="Store Country">
                            <Key>
                                <Column name="STORE_COUNTRY"/>
                            </Key>
                        </Attribute>
                        <Attribute hasHierarchy="false" keyColumn="STORE_STATE" name="Store State"/>
                        <Attribute hasHierarchy="false" name="Store City">
                            <Key>
                                <Column name="STORE_STATE"/>
                                <Column name="STORE_CITY"/>
                            </Key>
                            <Name>
                                <Column name="STORE_CITY"/>
                            </Name>
                        </Attribute>
                        <Attribute hasHierarchy="false" keyColumn="STORE_ID" name="Store Id"/>
                        <Attribute hasHierarchy="false" keyColumn="STORE_NAME" name="Store Name">
                            <Property attribute="Store Type"/>
                            <Property attribute="Store Manager"/>
                            <Property attribute="Store Sqft"/>
                            <Property attribute="Grocery Sqft"/>
                            <Property attribute="Frozen Sqft"/>
                            <Property attribute="Meat Sqft"/>
                            <Property attribute="Has coffee bar"/>
                            <Property attribute="Street address"/>
                        </Attribute>
                        <Attribute hierarchyAllMemberName="All Store Types" keyColumn="STORE_TYPE" name="Store Type"/>
                        <Attribute hasHierarchy="false" keyColumn="STORE_MANAGER" name="Store Manager"/>
                        <Attribute hasHierarchy="false" keyColumn="STORE_SQFT" name="Store Sqft"/>
                        <Attribute hasHierarchy="false" keyColumn="GROCERY_SQFT" name="Grocery Sqft"/>
                        <Attribute hasHierarchy="false" keyColumn="FROZEN_SQFT" name="Frozen Sqft"/>
                        <Attribute hasHierarchy="false" keyColumn="MEAT_SQFT" name="Meat Sqft"/>
                        <Attribute hasHierarchy="false" keyColumn="COFFEE_BAR" name="Has coffee bar"/>
                        <Attribute hasHierarchy="false" keyColumn="STORE_STREET_ADDRESS" name="Street address"/>
                    </Attributes>
                    <Hierarchies>
                        <Hierarchy allMemberName="All Stores" name="Stores">
                            <Level attribute="Store Country"/>
                            <Level attribute="Store State"/>
                            <Level attribute="Store City"/>
                            <Level attribute="Store Name"/>
                        </Hierarchy>
                        <Hierarchy name="Store Size in SQFT">
                            <Level attribute="Store Sqft"/>
                        </Hierarchy>
                    </Hierarchies>
                </Dimension>
                <Cube name="Store">
                    <Dimensions>
                        <Dimension key="Store Type" name="Store Type">
                            <Attributes>
                                <Attribute hasHierarchy="true" keyColumn="STORE_TYPE" name="Store Type" table="STORE"/>
                            </Attributes>
                        </Dimension>
                        <Dimension source="Store"/>
                        <Dimension name="Has coffee bar">
                            <Attributes>
                                <Attribute datatype="Boolean" hasHierarchy="true" keyColumn="COFFEE_BAR" name="Has coffee bar" table="STORE"/>
                            </Attributes>
                        </Dimension>
                    </Dimensions>
                    <MeasureGroups>
                        <MeasureGroup name="Store" table="STORE">
                            <Measures>
                                <Measure aggregator="sum" column="STORE_SQFT" formatString="#,###" name="Store Sqft"/>
                                <Measure aggregator="sum" column="GROCERY_SQFT" formatString="#,###" name="Grocery Sqft"/>
                            </Measures>
                            <DimensionLinks>
                                <FactLink dimension="Store"/>
                                <FactLink dimension="Store Type"/>
                                <FactLink dimension="Has coffee bar"/>
                            </DimensionLinks>
                        </MeasureGroup>
                    </MeasureGroups>
                </Cube>
            </Schema>

    1. login into saiku [webui](http://localhost:8080/) use `admin/admin`

    1. click `admin console` tab

    1. in `data source management` choose `add schema`

            # 1. choose your schema file
            # 2. input a schema name(e.g. foodmart_kylin)
            # 3. upload it

1. add data source

    1. login into saiku [webui](http://localhost:8080/) use `admin/admin`

    1. click `admin console` tab

    1. in `data source management` choose `add data source`

            name:             foodmart_kylin
            connection type:  mondrian
            url:              jdbc:kylin://localhost:7070/kylin_project
            schema:           /datasources/foodmart_kylin.xml
            jdbc driver:      org.apache.kylin.jdbc.Driver
            username:         ADMIN
            password:         ADMIN

    1. click `save`

### step 5 test query form saiku against kylin

1. login into saiku [webui](http://localhost:8080/) use `admin/admin`

1. click `new query` tab

1. drag drag drag

1. done

### reference

1. mustangore's github repo [kylin-mondrian-interaction](https://github.com/mustangore/kylin-mondrian-interaction)
