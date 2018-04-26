---
layout: post
title: "ansible playbook deploy svn code"
description: ""
category: [devops]
tags: [ansible, playbook, svn]
---
{% include JB/setup %}


### steps

1. deploy by ansible playbook

    1. yml task

            - name: checkout web code
                subversion:
                    repo={{ svn_repo }}
                    username={{ svn_user }}
                    password={{ svn_pass }}
                    dest="{{ working_dir }}/{{ svn_dest }}"
                    force=yes
                ignore_errors: yes

    1. certificate issued for a different hostname, issuer is not trusted

            fatal: [poc3]: FAILED! => {"changed": false, "cmd": "/usr/bin/svn --non-interactive --trust-server-cert --no-auth-cache --username myuser --password '********' checkout -r HEAD https://my-svn-ip:8443/svn/code /home/root/deploy/web", "failed": true, "msg": "svn: E175002: Unable to connect to a repository at URL 'https://my-svn-ip:8443/svn/code'\nsvn: E175002: OPTIONS of 'https://my-svn-ip:8443/svn/code': Server certificate verification failed: certificate issued for a different hostname, issuer is not trusted (https://my-svn-ip:8443)", "rc": 1, "stderr": "svn: E175002: Unable to connect to a repository at URL 'https://my-svn-ip:8443/svn/code'\nsvn: E175002: OPTIONS of 'https://my-svn-ip:8443/svn/code': Server certificate verification failed: certificate issued for a different hostname, issuer is not trusted (https://my-svn-ip:8443)\n", "stderr_lines": ["svn: E175002: Unable to connect to a repository at URL 'https://my-svn-ip:8443/svn/code'", "svn: E175002: OPTIONS of 'https://my-svn-ip:8443/svn/code': Server certificate verification failed: certificate issued for a different hostname, issuer is not trusted (https://my-svn-ip:8443)"], "stdout": "", "stdout_lines": []}
            ...ignoring


            # pretty format
            {
                "changed": false,
                "cmd": "/usr/bin/svn --non-interactive --trust-server-cert --no-auth-cache --username myuser --password '********' checkout -r HEAD https://my-svn-ip:8443/svn/code /home/root/deploy/web",
                "failed": true,
                "msg": "svn: E175002: Unable to connect to a repository at URL 'https://my-svn-ip:8443/svn/code'\nsvn: E175002: OPTIONS of 'https://my-svn-ip:8443/svn/code': Server certificate verification failed: certificate issued for a different hostname, issuer is not trusted (https://my-svn-ip:8443)",
                "rc": 1,
                "stderr": "svn: E175002: Unable to connect to a repository at URL 'https://my-svn-ip:8443/svn/code'\nsvn: E175002: OPTIONS of 'https://my-svn-ip:8443/svn/code': Server certificate verification failed: certificate issued for a different hostname, issuer is not trusted (https://my-svn-ip:8443)\n",
                "stderr_lines": [
                    "svn: E175002: Unable to connect to a repository at URL 'https://my-svn-ip:8443/svn/code'",
                    "svn: E175002: OPTIONS of 'https://my-svn-ip:8443/svn/code': Server certificate verification failed: certificate issued for a different hostname, issuer is not trusted (https://my-svn-ip:8443)"
                ],
                "stdout": "",
                "stdout_lines": []
            }


1. deploy manually

    1. method 1


            $ /usr/bin/svn --non-interactive --trust-server-cert --no-auth-cache --username myuser \
            --password mypass checkout -r HEAD https://my-svn-ip:8443/svn/code .

            # errors
            svn: E175002: 方法 OPTIONS 失败于 “https://my-svn-ip:8443/svn/code”: 服务器证书校验失败此证书是为不同的主机名称颁发, 证书发行者不被信任 (https://my-svn-ip:8443)


    1. method 2

            $ /usr/bin/svn --username myuser \
            --password mypass checkout -r HEAD https://my-svn-ip:8443/svn/code .

            验证“https://my-svn-ip:8443”的服务器证书时出错:
             - 此证书并不是由信任的权威机权颁发。请使用此指纹手工验证其有效性！
             - 证书的主机名称不匹配。
            证书信息:
             - 主机名称: WIN-UOICH30060A
             - 有效时间: 自 Mon, 13 Feb 2017 09:43:28 GMT 至 Thu, 11 Feb 2027 09:43:28 GMT
             - 发行者: WIN-UOICH30060A
             - 指纹: bd:6a:5a:7a:ba:3a:7a:3a:3a:9a:ea:1a:9a:4a:4a:9a:4a:da:aa:7a
            (R)拒绝，(t)暂时接受，或(p)永远接受？p

            -----------------------------------------------------------------------
            注意!  你的密码，对于认证域:

               <https://my-svn-ip:8443> VisualSVN Server

            只能明文保存在磁盘上!  如果可能的话，请考虑配置你的系统，让 Subversion
            可以保存加密后的密码。请参阅文档以获得详细信息。

            你可以通过在“/root/.subversion/servers”中设置选项“store-plaintext-passwords”为“yes”或“no”，
            来避免再次出现此警告。
            -----------------------------------------------------------------------
            保存未加密的密码(yes/no)?yes

### fixed

1. without parameters

    1. delete these parameters from cli

            --non-interactive --trust-server-cert --no-auth-cache

    1. checkout manually

            $ /usr/bin/svn --username myuser \
            --password mypass checkout -r HEAD https://my-svn-ip:8443/svn/code .

    1. config /root/.subversion/servers

            $ nano /root/.subversion/servers
            ...
            # store-plaintext-passwords = no
            store-plaintext-passwords = yes
            ...
