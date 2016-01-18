---
layout: post
title: "apache kylin get started"
description: ""
category: [bi]
tags: [olap]
---
{% include JB/setup %}


### fixed

1. kylin.log

    1. Failed to scan 

            Failed to scan [file:/contrib/capacity-scheduler/*.jar] from classloader hierarchy
            java.io.FileNotFoundException: /contrib/capacity-scheduler/*.jar (No such file or directory)
                at java.util.zip.ZipFile.open(Native Method)
                at java.util.zip.ZipFile.<init>(ZipFile.java:220)
                at java.util.zip.ZipFile.<init>(ZipFile.java:150)
                at java.util.jar.JarFile.<init>(JarFile.java:166)
                at java.util.jar.JarFile.<init>(JarFile.java:103)

1. localhost_access_log

            127.0.0.1 - - [18/Jan/2016:21:10:24 +0800] "GET /kylin HTTP/1.1" 404 -

1. localhost.log

            INFO: Initializing Spring root WebApplicationContext
            Jan 18, 2016 9:10:03 PM org.apache.catalina.core.StandardContext listenerStart
            SEVERE: Exception sending context initialized event to listener instance of class org.springframework.web.context.ContextLoaderListener
            org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping#0': BeanPostProcessor before instantiation of bean failed; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'org.springframework.cache.config.internalCacheAdvisor': Cannot resolve reference to bean 'org.springframework.cache.annotation.AnnotationCacheOperationSource#0' while setting bean property 'cacheOperationSource'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'org.springframework.cache.annotation.AnnotationCacheOperationSource#0': BeanPostProcessor before instantiation of bean failed; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'org.springframework.security.methodSecurityMetadataSourceAdvisor': Cannot resolve reference to bean 'org.springframework.security.access.method.DelegatingMethodSecurityMetadataSource#0' while setting constructor argument; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'org.springframework.security.access.method.DelegatingMethodSecurityMetadataSource#0': Cannot create inner bean '(inner bean)' of type [org.springframework.security.access.prepost.PrePostAnnotationSecurityMetadataSource] while setting constructor argument with key [0]; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name '(inner bean)': Cannot create inner bean '(inner bean)' of type [org.springframework.security.access.expression.method.ExpressionBasedAnnotationAttributeFactory] while setting constructor argument; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name '(inner bean)': Cannot resolve reference to bean 'expressionHandler' while setting constructor argument; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'expressionHandler' defined in class path resource [kylinSecurity.xml]: Cannot resolve reference to bean 'permissionEvaluator' while setting bean property 'permissionEvaluator'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'permissionEvaluator' defined in class path resource [kylinSecurity.xml]: Cannot resolve reference to bean 'aclService' while setting constructor argument; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'aclService' defined in file [/usr/local/apache-kylin/tomcat/webapps/kylin/WEB-INF/classes/org/apache/kylin/rest/service/AclService.class]: Instantiation of bean failed; nested exception is org.springframework.beans.BeanInstantiationException: Could not instantiate bean class [org.apache.kylin.rest.service.AclService]: Constructor threw exception; nested exception is java.lang.NoSuchMethodError: org.apache.hadoop.hbase.client.HBaseAdmin.<init>(Lorg/apache/hadoop/hbase/client/HConnection;)V
                at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:452)
                at org.springframework.beans.factory.support.AbstractBeanFactory$1.getObject(AbstractBeanFactory.java:294)
                at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:225)
                at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:291)
                at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:193)