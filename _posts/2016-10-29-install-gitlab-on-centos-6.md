---
layout: post
title: "install gitlab on centos 6"
description: ""
category: [setup]
tags: [gitlab, centos, wget, repo, yum, curl, openssh-clients, openssh-server, postfix, cronie, lokkit, chkconfig]
---
{% include JB/setup %}


### config yum

1. use aliyun repo

    1 backup

            $ cd /etc/yum.repos.d/
            $ sudo mv CentOS-Base.repo CentOS-Base.repo.backup

    1. config repo

            # centos 5
            $ cd /etc/yum.repos.d/
            $ sudo wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-5.repo

            # centos 6
            $ cd /etc/yum.repos.d/
            $ sudo wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo

    1. make cache

            $ sudo yum makecache

1. [install gitlab ce on centos 6 from gitlab](https://about.gitlab.com/downloads/#centos6)

    1. install and configure the necessary dependencies

            $ sudo yum install curl openssh-server openssh-clients postfix cronie
            $ sudo service postfix start
            $ sudo chkconfig postfix on
            $ sudo lokkit -s http -s ssh

    1. add the gitlab package server and install the package

            # method 1 via shell
            $ curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
            $ sudo yum install gitlab-ce

            # method 2 via rpm
            $ curl -LJO https://packages.gitlab.com/gitlab/gitlab-ce/packages/el/6/gitlab-ce-XXX.rpm/download
            $ rpm -i gitlab-ce-XXX.rpm

    1. config and start gitlab

            $ sudo gitlab-ctl reconfigure

1. [install gitlab ce on centos 6 from mirror tsinghua](https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/)

    1. create a new repo file

            $ sudo nano /etc/yum.repos.d/gitlab-ce.repo
            [gitlab-ce]
            name=gitlab-ce
            baseurl=http://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el6
            repo_gpgcheck=0
            gpgcheck=0
            enabled=1
            gpgkey=https://packages.gitlab.com/gpg.key

    1. run

            $ sudo yum makecache
            $ sudo yum install gitlab-ce

            Loaded plugins: fastestmirror, refresh-packagekit, security
            Setting up Install Process
            Loading mirror speeds from cached hostfile
             * base: mirrors.aliyun.com
             * epel: mirrors.tuna.tsinghua.edu.cn
             * extras: mirrors.aliyun.com
             * updates: mirrors.aliyun.com
            Resolving Dependencies
            --> Running transaction check
            ---> Package gitlab-ce.x86_64 0:8.13.1-ce.0.el6 will be installed
            --> Finished Dependency Resolution

            Dependencies Resolved

            =====================================================================================
             Package           Arch           Version                    Repository         Size
            =====================================================================================
            Installing:
             gitlab-ce         x86_64         8.13.1-ce.0.el6            gitlab-ce         281 M

            Transaction Summary
            =====================================================================================
            Install       1 Package(s)

            Total download size: 281 M
            Installed size: 281 M
            Is this ok [y/N]: y
            Downloading Packages:
            gitlab-ce-8.13.1-ce.0.el6.x86_64.rpm  | 281 MB     03:35     
            Running rpm_check_debug
            Running Transaction Test
            Transaction Test Succeeded
            Running Transaction
            Warning: RPMDB altered outside of yum.
              Installing : gitlab-ce-8.13.1-ce.0.el6.x86_64                                           1/1 
            hostname: Unknown host


                   *.                  *.
                  ***                 ***
                 *****               *****
                .******             *******
                ********            ********
               ,,,,,,,,,***********,,,,,,,,,
              ,,,,,,,,,,,*********,,,,,,,,,,,
              .,,,,,,,,,,,*******,,,,,,,,,,,,
                  ,,,,,,,,,*****,,,,,,,,,.
                     ,,,,,,,****,,,,,,
                        .,,,***,,,,
                            ,*,.

                 _______ __  __          __
                / ____(_) /_/ /   ____ _/ /_
               / / __/ / __/ /   / __ `/ __ \
              / /_/ / / /_/ /___/ /_/ / /_/ /
              \____/_/\__/_____/\__,_/_.___/


            gitlab: Thank you for installing GitLab!
            gitlab: To configure and start GitLab, RUN THE FOLLOWING COMMAND:

            sudo gitlab-ctl reconfigure

            gitlab: GitLab should be reachable at http://gitlab.example.com
            gitlab: Otherwise configure GitLab for your system by editing /etc/gitlab/gitlab.rb file
            gitlab: And running reconfigure again.
            gitlab: 
            gitlab: For a comprehensive list of configuration options please see the Omnibus GitLab readme
            gitlab: https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/README.md
            gitlab: 
            It looks like GitLab has not been configured yet; skipping the upgrade script.
              Verifying  : gitlab-ce-8.13.1-ce.0.el6.x86_64                                           1/1 

            Installed:
              gitlab-ce.x86_64 0:8.13.1-ce.0.el6                                                          

            Complete!
            You have new mail in /var/spool/mail/root

### fixed

1. Cannot retrieve repository metadata (repomd.xml) for repository: rbel5

    1. error info

            $ sudo yum makecache
            ...   
            http://rbel.frameos.org/stable/el5/x86_64/repodata/repomd.xml: [Errno 12] Timeout on http://rbel.frameos.org/stable/el5/x86_64/repodata/repomd.xml: (28, 'connect() timed out!')
            Trying other mirror.
            Error: Cannot retrieve repository metadata (repomd.xml) for repository: rbel5. Please verify its path and try again

    1. fixed

            $ cd /etc/yum.repos.d/
            $ sudo mv rbel5.repo rbel5.repo.backup

            $ sudo yum makecache
            Loaded plugins: fastestmirror, refresh-packagekit, security
            Loading mirror speeds from cached hostfile
            epel/metalink   | 5.2 kB     00:00     
             * base: mirrors.aliyun.com
             * epel: mirrors.tuna.tsinghua.edu.cn
             * extras: mirrors.aliyun.com
             * updates: mirrors.aliyun.com
            WANdisco-git    |  951 B     00:00     
            base            | 3.7 kB     00:00     
            extras          | 3.4 kB     00:00     
            gitlab-ce       | 2.9 kB     00:00     
            updates         | 3.4 kB     00:00     
            updates/filelists_db    | 2.1 MB     00:01     
            updates/prestodelta     | 225 kB     00:00     
            updates/primary_db      | 3.1 MB     00:02     
            updates/other_db        |  41 MB     00:29     
            Metadata Cache Created
