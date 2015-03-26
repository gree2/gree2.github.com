---
layout: post
title: "batch automate ftp installation and ftp site creation"
description: ""
category: [batch]
tags: [ftp, iis, pkgmgr]
---
{% include JB/setup %}

### install iis and ftp roles

1. [reference](http://www.vsysad.com/2013/05/install-iis-7-5-ftp-server-from-the-command-line-or-script/)

2. batch file

        CMD /C START /w PKGMGR.EXE /l:log.etw /iu:IIS-WebServerRole;^
        IIS-WebServer;^
        IIS-CommonHttpFeatures;^
        IIS-StaticContent;^
        IIS-DefaultDocument;^
        IIS-DirectoryBrowsing;^
        IIS-HttpErrors;^
        IIS-HttpRedirect;^
        IIS-ApplicationDevelopment;^
        IIS-ASPNET;^
        IIS-NetFxExtensibility;^
        IIS-CGI;^
        IIS-ISAPIExtensions;^
        IIS-ISAPIFilter;^
        IIS-HealthAndDiagnostics;^
        IIS-HttpLogging;^
        IIS-LoggingLibraries;^
        IIS-RequestMonitor;^
        IIS-HttpTracing;^
        IIS-Security;^
        IIS-BasicAuthentication;^
        IIS-WindowsAuthentication;^
        IIS-URLAuthorization;^
        IIS-RequestFiltering;^
        IIS-IPSecurity;^
        IIS-Performance;^
        IIS-HttpCompressionStatic;^
        IIS-HttpCompressionDynamic;^
        IIS-WebServerManagementTools;^
        IIS-ManagementConsole;^
        IIS-ManagementScriptingTools;^
        IIS-ManagementService;^
        IIS-IIS6ManagementCompatibility;^
        IIS-Metabase;^
        IIS-WMICompatibility;^
        IIS-LegacyScripts;^
        IIS-LegacySnapIn;^
        IIS-FTPServer;^
        IIS-FTPSvc;^
        IIS-FTPExtensibility;^
        WAS-WindowsActivationService;^
        WAS-ProcessModel;^
        WAS-NetFxEnvironment;^
        WAS-ConfigurationAPI;^
        IIS-ManagementService

### setup ftp site

1. [reference](http://blogs.iis.net/kehand/archive/2009/08/09/automate-ftp-7-5-installation-and-ftp-site-creation.aspx)

1. batch

        cd %windir%\system32\inetsrv
        set ftpsite=Default Ftp Site
        set ftproot=%systemdrive%\inetpub\wwwroot
        appcmd add site /name:"%ftpsite%" /bindings:ftp://*:21 /physicalpath:"%ftproot%" 
        appcmd set config -section:system.applicationHost/sites "/[name='%ftpsite%'].ftpServer.security.ssl.controlChannelPolicy:SslAllow"
        appcmd set config -section:system.applicationHost/sites "/[name='%ftpsite%'].ftpServer.security.ssl.dataChannelPolicy:SslAllow"
        appcmd set config -section:system.applicationHost/sites "/[name='%ftpsite%'].ftpServer.security.authentication.basicAuthentication.enabled:true"
        appcmd set config -section:system.applicationHost/sites "/[name='%ftpsite%'].ftpServer.security.authentication.anonymousAuthentication.enabled:true"
        @REM Before adding all user read/write permission, remove all users in case it's there already
        appcmd set config "%ftpsite%" /section:system.ftpserver/security/authorization /-[users='*'] /commit:apphost
        appcmd set config "%ftpsite%" /section:system.ftpserver/security/authorization /+[accessType='Allow',permissions='Read,Write',roles='',users='ftp_admin'] /commit:apphost