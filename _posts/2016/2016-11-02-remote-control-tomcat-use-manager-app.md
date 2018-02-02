---
layout: post
title: "remote control tomcat use manager app"
description: ""
category: [devops]
tags: [tomcat, manager, app, manager-gui]
---
{% include JB/setup %}


### setup

1. edit `tomcat-user.xml`

    1. add these code

            <role rolename="admin"/>
            <role rolename="admin-gui"/>
            <role rolename="admin-script"/>
            <role rolename="manager-gui"/>
            <role rolename="manager-jmx"/>
            <role rolename="manager-script"/>
            <role rolename="manager-status"/>

            <user username="admin" password="admin" roles="manager-gui,manager-script,manager-jmx,manager-status,admin-gui,admin-script"/>

1. edit `$CATALINA_BASE/conf/localhost/manager.xml`

    1. [add these code](http://stackoverflow.com/questions/36703856/access-tomcat-manager-app-from-different-host)

            <Context privileged="true" antiResourceLocking="false"
                     docBase="${catalina.home}/webapps/manager">
                <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$" />
            </Context>

1. visit [http://localhost:8080/manager/html](http://localhost:8080/manager/html)
