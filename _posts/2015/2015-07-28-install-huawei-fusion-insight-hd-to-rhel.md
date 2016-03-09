---
layout: post
title: "install huawei fusion insight hd to rhel"
description: ""
category: [bigdata]
tags: [huawei, fusion, insight, hd, rhel]
---
{% include JB/setup %}


### oms

1. install

            $ cd FusionInsight
            $ ls
            ProductDoc  ReleaseDoc  software
            $ cd software/
            $ ls
            component  install.ini  install.sh  om  precheck  preinstall  userid.ini
            $ ./install.sh -m single
            =================================== Welcome ===================================
            === STEP 1 Preparing components...
            === STEP 2 Install packages. It may take several minutes. Please wait...
            ERROR:Failed to install OMS installation package.
            ERROR:Failed to install oms.
            ERROR:Installation failed.
                  Please run the following script to delete useless files:
                  /opt/huawei/Bigdata/om-0.0.1/inst/uninstall.sh

1. uninstall

            $ /opt/huawei/Bigdata/om-0.0.1/inst/uninstall.sh
            ------------------------------------------------------------------------------------------
            Prerequisites:
            1. The system has no clusters. If clusters have been installed, uninstall them.
            2. No operation is being performed.
            Check that all conditions are met before the uninstallation.
            ------------------------------------------------------------------------------------------
            Are all the conditions met? (y/n):y
            start to uninstall okerberos.
            start to uninstall oldap.
            Uninstallation in progress.
            Stopping the OMS...
            Uninstalling the OMS...
            Uninstalling the OMA...
            Uninstalltion succeeded.
            Uninstallation is successful.
