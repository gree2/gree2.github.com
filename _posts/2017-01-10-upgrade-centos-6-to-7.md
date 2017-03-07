---
layout: post
title: "upgrade centos 6 to 7"
description: ""
category: [linux]
tags: [centos, upgrade, 6, 7, createrepo, gpgcheck]
---
{% include JB/setup %}


### steps

1. use redhat upgrade tool


    1. create repo file

            # vi /etc/yum.repos.d/upgrade.repo

            [upgrade]
            name=upgrade
            baseurl=http://dev.centos.org/centos/6/upg/x86_64/
            enabled=1
            gpgcheck=0

    1. install packages

            # yum -y install preupgrade-assistant-contents redhat-upgrade-tool preupgrade-assistant

    1. preupgrade assistant

            # preupg

            Preupg tool doesn't do the actual upgrade.
            Please ensure you have backed up your system and/or data in the event of a failed upgrade
             that would require a full re-install of the system from installation media.
            Do you want to continue? y/n
            y
            Gathering logs used by preupgrade assistant:
            All installed packages : 01/11 ...finished (time 00:00s)
            All changed files      : 02/11 ...finished (time 01:00s)
            Changed config files   : 03/11 ...finished (time 00:00s)
            All users              : 04/11 ...finished (time 00:00s)
            All groups             : 05/11 ...finished (time 00:00s)
            Service statuses       : 06/11 ...finished (time 00:00s)
            All installed files    : 07/11 ...finished (time 00:01s)
            All local files        : 08/11 ...finished (time 00:03s)
            All executable files   : 09/11 ...finished (time 00:01s)
            RedHat signed packages : 10/11 ...finished (time 00:00s)
            CentOS signed packages : 11/11 ...finished (time 00:00s)
            Assessment of the system, running checks / SCE scripts:
            001/096 ...done    (Configuration Files to Review)
            ...

    1. failed...

1. use iso

    1. download

            # nohup wget -c -b -q http://mirrors.163.com/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1611.iso &

    1. recipe from [redhat s1 yum upgrade system](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Deployment_Guide/s1-yum-upgrade-system.html)

            1. create mount dir
            # mkdir centos7

            2. mount iso image
            # mount -o loop CentOS-7-x86_64-DVD-1611.iso centos7

            3. check first line of .discinfo
            # head -n1 centos7/.discinfo
            1480943823.812754
            # media_id  <= this is the output

            4. create `/etc/yum.repos.d/centos7.repo
            # nano /etc/yum.repos.d/centos7.repo
            [repository] 
            mediaid=media_id 
            name=centos7-iso
            baseurl=file:///media/centos/Server
            gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-??? 
            enabled=1 
            gpgcheck=1

            5. update all yum repo
            # yum update

            this upgrades system to the version provided by the mounted iso image

            6. unmount iso image
            # umount centos7
            # rmdir centos7

            7. remove as you wish
            # rm /etc/yum.repos.d/centos7.repo

    1. recipe from [how upgrade centos 5.7 using centos 5.8 iso image](http://www.unix.com/red-hat/197473-how-upgrade-centos-5-7-using-centos-5-8-iso-image-vmware-workstation.html)

            1.
            # mkdir -p /var/www/html/dvd/iso
            # mount -o loop CentOS-7-x86_64-DVD-1611.iso /var/www/html/dvd/iso
            # cd /var/www/html/dvd
            # createrepo .
            # yum clean all

            2.
            # nano /etc/yum.repos.d/centos7.repo
            [centos7]
            name=centos7
            baseurl=file:///var/www/html/dvd
            enabled=1
            gpgcheck=0

            3.
            # yum list
            # yum upgrade

### fixs

1. I/O warning

    1. info

            096/096 ...done    (NIS server config file back-up)
            Assessment finished (time 05:01s)
            I/O warning : failed to load external entity "/usr/share/openscap/xsl/security-guide.xsl"
            compilation error: file /usr/share/preupgrade/xsl/preup.xsl line 40 element import
            xsl:import : unable to load /usr/share/openscap/xsl/security-guide.xsl
            I/O warning : failed to load external entity "/usr/share/openscap/xsl/oval-report.xsl"
            compilation error: file /usr/share/preupgrade/xsl/preup.xsl line 41 element import
            xsl:import : unable to load /usr/share/openscap/xsl/oval-report.xsl
            I/O warning : failed to load external entity "/usr/share/openscap/xsl/sce-report.xsl"
            compilation error: file /usr/share/preupgrade/xsl/preup.xsl line 42 element import
            xsl:import : unable to load /usr/share/openscap/xsl/sce-report.xsl
            OpenSCAP Error:: Could not parse XSLT file '/usr/share/preupgrade/xsl/preup.xsl' [oscapxml.c:416]
            Unable to open file /root/preupgrade/result.html
            Usage: preupg [options]

            preupg: error: [Errno 2] No such file or directory: '/root/preupgrade/result.html'

    1. [CentOSUpgradeTool](https://wiki.centos.org/TipsAndTricks/CentOSUpgradeTool)

            DO NOT USE this tool. Warning: use of this tool is currently BROKEN as several system-critical packages are of a higher version number in CentOS 6.7 than they are in CentOS 7 so those do not get upgraded correctly. This renders yum and several other system tools non-functional.

1. yum upgrade error

    1. info

            You could try using --skip-broken to work around the problem
            # yum upgrade --skip-broken

    1. error again

            Multilib version problems found. This often means that the root
                  cause is something else and multilib version checking is just
                  pointing out that there is a problem. Eg.:

                    1. You have an upgrade for libgcc which is missing some
                       dependency that another package requires. Yum is trying to
                       solve this by installing an older version of libgcc of the
                       different architecture. If you exclude the bad architecture
                       yum will tell you what the root cause is (which package
                       requires what). You can try redoing the upgrade with
                       --exclude libgcc.otherarch ... this should give you an error
                       message showing the root cause of the problem.

                    2. You have multiple architectures of libgcc installed, but
                       yum can only see an upgrade for one of those arcitectures.
                       If you don't want/need both architectures anymore then you
                       can remove the one with the missing update and everything
                       will work.

                    3. You have duplicate versions of libgcc installed already.
                       You can use "yum check" to get yum show these errors.

                  ...you can also use --setopt=protected_multilib=false to remove
                  this checking, however this is almost never the correct thing to
                  do as something else is very likely to go wrong (often causing
                  much more problems).

                  Protected multilib versions: libgcc-4.8.5-11.el7.x86_64 != libgcc-4.4.7-17.el6.x86_64
