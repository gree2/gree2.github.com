---
layout: post
title: "docker on centos"
description: ""
category: [docker]
tags: [centos, install, yum, docker-engine, docker-io epel]
---
{% include JB/setup %}


### install docker-engine

1. install with yum

    1. login with sudo or root privileges

    1. make sure your existing packages are up-to-date.

            $ sudo yum update

    1. add the yum repo

            $ sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
            [dockerrepo]
            name=Docker Repository
            baseurl=https://yum.dockerproject.org/repo/main/centos/7/
            enabled=1
            gpgcheck=1
            gpgkey=https://yum.dockerproject.org/gpg
            EOF

    1. install the docker package

            $ sudo yum install docker-engine

    1. enable the service

            $ sudo systemctl enable docker.service

    1. start the docker daemon

            $ sudo systemctl start docker

    1. verify

            $ sudo docker run --rm hello-world

1. install with the script

    1. login with sudo or root privileges

    1. make sure your existing packages are up-to-date

            $ sudo yum update

    1. run the docker installation script

            $ curl -fsSL https://get.docker.com/ | sh

    1. enable the service

            $ sudo systemctl enable docker.service

    1. start the docker daemon

            $ sudo systemctl start docker

    1. verify
    
            $ sudo docker run --rm hello-world

1. install docker-io (old version)

    1. install the epel repo

            $ rpm -iUvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

    1. update our packages:

            $ yum update -y

    1. docker-io package:

            $ yum -y install docker-io

    1. start the docker daemon

            $ service docker start

    1. config start with the server boots

            $ chkconfig docker on

### fixed

1. error

    1. message

            Error: Package: docker-engine-1.12.5-1.el7.centos.x86_64 (dockerrepo)
                       Requires: device-mapper-libs >= 1.02.90-1
                       Installed: device-mapper-libs-1.02.79-8.el6.x86_64 (@anaconda-CentOS-201311291202.x86_64/6.5)
                           device-mapper-libs = 1.02.79-8.el6
            Error: Package: docker-engine-1.12.5-1.el7.centos.x86_64 (dockerrepo)
                       Requires: libsystemd.so.0(LIBSYSTEMD_209)(64bit)
            Error: Package: docker-engine-1.12.5-1.el7.centos.x86_64 (dockerrepo)
                       Requires: libseccomp.so.2()(64bit)
            Error: Package: docker-engine-1.12.5-1.el7.centos.x86_64 (dockerrepo)
                       Requires: libsystemd.so.0()(64bit)
            Error: Package: docker-engine-1.12.5-1.el7.centos.x86_64 (dockerrepo)
                       Requires: libc.so.6(GLIBC_2.17)(64bit)
            Error: Package: docker-engine-1.12.5-1.el7.centos.x86_64 (dockerrepo)
                       Requires: selinux-policy >= 3.13.1-23
                       Installed: selinux-policy-3.7.19-231.el6.noarch (@anaconda-CentOS-201311291202.x86_64/6.5)
                           selinux-policy = 3.7.19-231.el6
            Error: Package: docker-engine-1.12.5-1.el7.centos.x86_64 (dockerrepo)
                       Requires: libdevmapper.so.1.02(DM_1_02_97)(64bit)
            Error: Package: docker-engine-1.12.5-1.el7.centos.x86_64 (dockerrepo)
                       Requires: systemd-units
            Error: Package: docker-engine-selinux-1.12.5-1.el7.centos.noarch (dockerrepo)
                       Requires: selinux-policy-base >= 3.13.1-23
                       Installed: selinux-policy-targeted-3.7.19-231.el6.noarch (@anaconda-CentOS-201311291202.x86_64/6.5)
                           selinux-policy-base = 3.7.19-231.el6
                       Available: selinux-policy-minimum-3.7.19-231.el6.noarch (os)
                           selinux-policy-base = 3.7.19-231.el6
                       Available: selinux-policy-mls-3.7.19-231.el6.noarch (os)
                           selinux-policy-base = 3.7.19-231.el6
            Error: Package: docker-engine-selinux-1.12.5-1.el7.centos.noarch (dockerrepo)
                       Requires: selinux-policy-targeted >= 3.13.1-23
                       Installed: selinux-policy-targeted-3.7.19-231.el6.noarch (@anaconda-CentOS-201311291202.x86_64/6.5)
                           selinux-policy-targeted = 3.7.19-231.el6
             You could try using --skip-broken to work around the problem
             You could try running: rpm -Va --nofiles --nodigest

    1. use --skip-broken

            $ yum install docker-engine --skip-broken
            ...
            Packages skipped because of dependency problems:
                audit-libs-python-2.2-2.el6.x86_64 from os
                docker-engine-1.12.5-1.el7.centos.x86_64 from dockerrepo
                docker-engine-selinux-1.12.5-1.el7.centos.noarch from dockerrepo
                libselinux-python-2.0.94-5.3.el6_4.1.x86_64 from os
                libsemanage-python-2.0.43-4.2.el6.x86_64 from os
                libtool-ltdl-2.2.6-15.5.el6.x86_64 from os
                policycoreutils-python-2.0.83-19.39.el6.x86_64 from os
                setools-libs-3.3.7-4.el6.x86_64 from os
                setools-libs-python-3.3.7-4.el6.x86_64 from os

1. final fixed

    1. [docs for installation on linux centos](https://docs.docker.com/engine/installation/linux/centos/)'s prerequisites Docker requires a `64-bit OS` and version `3.10 or higher` of the `Linux kernel`

    1. check my post [2016-12-23-upgrade-centos-6.5-kernel-from-2.6-to-3.10]({% post_url 2016-12-23-upgrade-centos-65-kernel-from-26-to-310 %})

            # you should upgrade os kernel first
