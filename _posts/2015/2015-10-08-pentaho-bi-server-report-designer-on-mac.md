---
layout: post
title: "pentaho bi server report designer on mac"
description: ""
category: [bi]
tags: [bi, pentaho]
---
{% include JB/setup %}


### setup

1. download

    1. [pentaho bi server](http://jaist.dl.sourceforge.net/project/pentaho/Business%20Intelligence%20Server/5.4/biserver-ce-5.4.0.1-130.zip)

    1. [pentaho report designer](http://jaist.dl.sourceforge.net/project/pentaho/Report%20Designer/5.4/prd-ce-mac-5.4.0.1-130.zip)

1. usage

    1. start up pentaho bi server

            $ cd path/to/biserver-ce
            $ $ ./start-pentaho.sh
            DEBUG: Using JAVA_HOME
            DEBUG: _PENTAHO_JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home
            DEBUG: _PENTAHO_JAVA=/Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home/bin/java
            --------------------------------------------------------------------------------------------
            The Pentaho BI Platform now contains a version checker that will notify you
            when newer versions of the software are available. The version checker is enabled by default.
            For information on what the version checker does, why it is beneficial, and how it works see:
            http://wiki.pentaho.com/display/ServerDoc2x/Version+Checker
            Press Enter to continue, or type cancel or Ctrl-C to prevent the server from starting.
            You will only be prompted once with this question.
            --------------------------------------------------------------------------------------------
            [OK]:

            Using CATALINA_BASE:   /Users/hqlgree2/Downloads/pentaho/biserver-ce/tomcat
            Using CATALINA_HOME:   /Users/hqlgree2/Downloads/pentaho/biserver-ce/tomcat
            Using CATALINA_TMPDIR: /Users/hqlgree2/Downloads/pentaho/biserver-ce/tomcat/temp
            Using JRE_HOME:        /Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home
            Using CLASSPATH:       /Users/hqlgree2/Downloads/pentaho/biserver-ce/tomcat/bin/bootstrap.jar

    1. visit [http://localhost:8080](http://localhost:8080)

    1. visit [http://localhost:8099](http://localhost:8099)

### create mysql demo

1. check `README.TXT`

            $ cd biserver-ce/data
            $ cat README.TXT

            For mysql5, oracle10g, and postgresql Installs, run the sql create scripts in
            the following order:

            create_repository_<db>.sql
            create_quartz_<db>.sql

            If you would like to create the sample data source connection, run the following script after running the create script:

            create_sample_datasource_<db>.sql

            If you want to use an existing 1.7.0 Pentaho rdbms repository database, run the following script:

            migration.sql

1. run sql script

        $ cd biserver-ce/data/mysql5
        $ mysql -u root -p
        mysql> source create_repository_mysql.sql;
        mysql> source create_quartz_mysql.sql;
        mysql> source create_jcr_mysql.sql;

1. config to use mysql

    1. edit `applicationContext-spring-security-hibernate.properties`

            $ cd biserver-ce/pentaho-solutions/system
            $ pico applicationContext-spring-security-hibernate.properties

            # change
            jdbc.driver=org.hsqldb.jdbcDriver
            jdbc.url=jdbc:hsqldb:hsql://localhost:9001/hibernate
            jdbc.username=hibuser
            jdbc.password=password
            hibernate.dialect=org.hibernate.dialect.HSQLDialect

            # to
            jdbc.driver=com.mysql.jdbc.Driver
            jdbc.url=jdbc:mysql://localhost:3306/hibernate
            jdbc.username=root
            jdbc.password=root
            hibernate.dialect=org.hibernate.dialect.MySQLDialect

    1. edit `applicationContext-spring-security-jdbc.xml`

            $ cd biserver-ce/pentaho-solutions/system
            $ pico applicationContext-spring-security-jdbc.xml

            # change
            <!--  This is only for Hypersonic. Please update this section for any other database you are using -->  
            <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
                <property name="driverClassName" value="org.hsqldb.jdbcDriver" />
                <property name="url" value="jdbc:hsqldb:hsql://localhost:9001/hibernate" />
                <property name="username" value="hibuser" />
                <property name="password" value="password" />
            </bean>

            <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
                <property name="driverClassName" value="${datasource.driver.classname}" />
                <property name="url" value="${datasource.url}" />
                <property name="username" value="${datasource.username}" />
                <property name="password" value="${datasource.password}" />
                <!-- the following are optional -->
                <property name="validationQuery" value="${datasource.validation.query}" />
                <property name="maxWait" value="${datasource.pool.max.wait}" />
                <property name="maxActive" value="${datasource.pool.max.active}" />
                <property name="maxIdle" value="${datasource.max.idle}" />
                <property name="minIdle" value="${datasource.min.idle}" />
            </bean>


            # to
            <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
                <property name="driverClassName" value="com.mysql.jdbc.Driver" />
                <property name="url" value="jdbc:mysql://localhost:3306/hibernate" />
                <property name="username" value="hibuser" />
                <property name="password" value="password"/>
                <!-- the following are optional -->
                <property name="validationQuery" value="${datasource.validation.query}" />
                <property name="maxWait" value="${datasource.pool.max.wait}" />
                <property name="maxActive" value="${datasource.pool.max.active}" />
                <property name="maxIdle" value="${datasource.max.idle}" />
                <property name="minIdle" value="${datasource.min.idle}" />
            </bean>

    1. edit `mysql5.hibernate.cfg.xml`

            $ cd biserver-ce/pentaho-solutions/system/hibernate
            $ pico mysql5.hibernate.cfg.xml

            # check username and password
            <property name="connection.username">hibuser</property>
            <property name="connection.password">password</property>

    1. edit `hibernate-settings.xml`

            $ cd biserver-ce/pentaho-solutions/system/hibernate
            $ pico hibernate-settings.xml

            # change
            <config-file>system/hibernate/hsql.hibernate.cfg.xml</config-file>

            # to
            <config-file>system/hibernate/mysql5.hibernate.cfg.xml</config-file>

    1. edit `context.xml`

            $ cd biserver-ce/tomcat/webapps/pentaho/META-INF
            $ pico context.xml

            # change
            <?xml version="1.0" encoding="UTF-8"?>
            <Context path="/pentaho" docbase="webapps/pentaho/">
                <Resource name="jdbc/Hibernate" auth="Container" type="javax.sql.DataSource"
                factory="org.apache.commons.dbcp.BasicDataSourceFactory" maxActive="20" maxIdle="5"
                maxWait="10000" username="hibuser" password="password"
                driverClassName="com.mysql.jdbc.Driver" url="jdbc:mysql://localhost:3306/hibernate"
                validationQuery="select 1" />
                <Resource name="jdbc/Quartz" auth="Container" type="javax.sql.DataSource"
                factory="org.apache.commons.dbcp.BasicDataSourceFactory" maxActive="20" maxIdle="5"
                maxWait="10000" username="pentaho_user" password="password"
                driverClassName="com.mysql.jdbc.Driver" url="jdbc:mysql://localhost:3306/quartz"
                validationQuery="select 1"/>
            </Context>

            # to
            <?xml version="1.0" encoding="UTF-8"?>
            <Context path="/pentaho" docbase="webapps/pentaho/">
                <Resource name="jdbc/Hibernate" auth="Container" type="javax.sql.DataSource"
                factory="org.apache.commons.dbcp.BasicDataSourceFactory" maxActive="20" maxIdle="5"
                maxWait="10000" username="root" password="root"
                driverClassName="com.mysql.jdbc.Driver" url="jdbc:mysql://localhost:3306/hibernate"
                validationQuery="select 1" />
                <Resource name="jdbc/Quartz" auth="Container" type="javax.sql.DataSource"
                factory="org.apache.commons.dbcp.BasicDataSourceFactory" maxActive="20" maxIdle="5"
                maxWait="10000" username="root" password="root"
                driverClassName="com.mysql.jdbc.Driver" url="jdbc:mysql://localhost:3306/quartz"
                validationQuery="select 1"/>
            </Context>

    1. edit `web.xml`

            $ cd biserver-ce/tomcat/webapps/pentaho/WEB_INF
            $ pico web.xml

            # change
            <context-param>
                <param-name>solution-path</param-name>
                <param-value></param-value>
            </context-param>

            # to
            <context-param>
                <param-name>solution-path</param-name>
                <param-value>/Users/hqlgree2/Downloads/pentaho/biserver-ce/pentaho-solutions</param-value>
            </context-param>

    1. edit `publisher_config.xml`

            $ cd biserver-ce/pentaho-solutions/system
            $ pico publisher_config.xml

            # change
            <publisher-config>  
                <publisher-password></publisher-password>  
            </publisher-config> 

            # to
            <publisher-config>  
                <publisher-password>password</publisher-password>  
            </publisher-config>

### fixed

1. visit [Pentaho Initialization Exception](http://localhost:8080/)

    1. Pentaho Initialization Exception

            The following errors were detected
            One or more system listeners failed. These are set in the systemListeners.xml.
               org.pentaho.platform.api.engine.PentahoSystemException: PentahoSystem.ERROR_0014 - Error while trying to execute startup sequence for org.pentaho.platform.repository2.unified.BackingRepositoryLifecycleManagerSystemListener

            Please see the server console for more details on each error detected.

            1) Stop BI server
            2) Flushed the Privileges of QuartZ database , created new user and password for QuartZ database and included new credentials at context.xml under /biserver-ce/tomcat/webapps/pentaho/META-INF/ folder
            3) Re-start BI server
            4) Try accessing your PUC

1. setup pentaho bi server in ubuntu

    1. org.h2.jdbc.JdbcSQLException

            org.h2.jdbc.JdbcSQLException: Exception opening port "H2 TCP Server (tcp://localhost:9092)" (port may be in use)

    1. [fixed](http://pentahochina.com/forum.php?mod=viewthread&tid=2618)

            $ pico start-pentaho.sh
            -Dh2.bindAddress=localhost

    1. [fixed](http://m.oschina.net/question/54100_39411)

            $ sudo ifdown eth0
            $ sudo service networking restart

1. chart encoding error

    1. [www.cnblogs.com](http://www.cnblogs.com/mybi/archive/2012/09/07/2676132.html)

    1. [zhanghaoeye](http://zhanghaoeye.iteye.com/blog/708211)

    1. [www.linuxidc.com](http://www.linuxidc.com/Linux/2009-02/18279.htm) fixed

            # 1. jre fonts
            $ cd $JAVA_HOME
            $ cd jre/lib/fonts
            $ sudo mkdir fallback
            $ ls -l
            total 2048
            -rw-r--r-- 1 uucp 143   75144 Apr 11  2015 LucidaBrightDemiBold.ttf
            -rw-r--r-- 1 uucp 143   75124 Apr 11  2015 LucidaBrightDemiItalic.ttf
            -rw-r--r-- 1 uucp 143   80856 Apr 11  2015 LucidaBrightItalic.ttf
            -rw-r--r-- 1 uucp 143  344908 Apr 11  2015 LucidaBrightRegular.ttf
            -rw-r--r-- 1 uucp 143  317896 Apr 11  2015 LucidaSansDemiBold.ttf
            -rw-r--r-- 1 uucp 143  698236 Apr 11  2015 LucidaSansRegular.ttf
            -rw-r--r-- 1 uucp 143  234068 Apr 11  2015 LucidaTypewriterBold.ttf
            -rw-r--r-- 1 uucp 143  242700 Apr 11  2015 LucidaTypewriterRegular.ttf
            drwxr-xr-x 2 root root   4096 Oct 13 17:32 fallback
            -rw-r--r-- 1 uucp 143    4041 Apr 11  2015 fonts.dir

            $ sudo chown uucp.143 fallback
            $ sudo cp /home/node/.fonts/msyh.ttf fallback/
            $ sudo chown uucp.143 fallback/msyh.ttf

            # 2. user fonts
            $ cd
            $ mkdir .fonts
            $ cp msyh.ttf .fonts/
            $ fc-cache -fv .fonts/
