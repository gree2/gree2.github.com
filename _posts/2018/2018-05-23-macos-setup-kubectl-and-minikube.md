---
layout: post
title: "macos setup kubectl and minikube"
description: ""
category: [setup]
tags: [kubectl, minikube]
---
{% include JB/setup %}


### steps

1. [install kuberctl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

    1. use brew

            $ brew install kubectl
            ...
            ==> Summary
            🍺  /usr/local/Cellar/kubernetes-cli/1.10.2: 178 files, 52.8MB

1. [install minikube](https://github.com/kubernetes/minikube/releases)

    1. use curl

            $ curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.27.0/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

    1. version

            $ minikube version
            minikube version: v0.27.0

    1. start a kubernets with minikube

            $ minikube start
            Starting local Kubernetes v1.10.0 cluster...
            Starting VM...
            Getting VM IP address...
            Moving files into cluster...
            Setting up certs...
            Connecting to cluster...
            Setting up kubeconfig...
            Starting cluster components...
            Kubectl is now configured to use the cluster.
            Loading cached images from config file.

    1. check status

            $ minikube status
            minikube: Running
            cluster: Running
            kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.100

    1. reset and restart cluster

            $ minikube delete
            $ minikube start

    1. dashboard

            $ minikube dashboard
            Opening kubernetes dashboard in default browser...
            http://192.168.99.100:30000/

1. kubectl

    1. version

            $ kubectl version
            Client Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.2", GitCommit:"81753b10df112992bf51bbc2c2f85208aad78335", GitTreeState:"clean", BuildDate:"2018-05-12T04:12:12Z", GoVersion:"go1.9.6", Compiler:"gc", Platform:"darwin/amd64"}
            Server Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.0", GitCommit:"fc32d2f3698e36b93322a3465f63a14e9f0eaead", GitTreeState:"clean", BuildDate:"2018-03-26T16:44:10Z", GoVersion:"go1.9.3", Compiler:"gc", Platform:"linux/amd64"}

    1. cluster-info

            $ kubectl cluster-info
            Kubernetes master is running at https://192.168.99.100:8443
            KubeDNS is running at https://192.168.99.100:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

            To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

    1. componentstatuses

            $ kubectl get componentstatuses
            NAME                 STATUS    MESSAGE              ERROR
            scheduler            Healthy   ok
            controller-manager   Healthy   ok
            etcd-0               Healthy   {"health": "true"}

    1. state of all pods

            $ kubectl get pods --all-namespaces
            NAMESPACE     NAME                                    READY     STATUS    RESTARTS   AGE
            kube-system   etcd-minikube                           1/1       Running   0          22m
            kube-system   kube-addon-manager-minikube             1/1       Running   0          21m
            kube-system   kube-apiserver-minikube                 1/1       Running   0          21m
            kube-system   kube-controller-manager-minikube        1/1       Running   0          21m
            kube-system   kube-dns-86f4d74b45-gtn4r               3/3       Running   1          21m
            kube-system   kube-proxy-f6s6b                        1/1       Running   0          21m
            kube-system   kube-scheduler-minikube                 1/1       Running   0          21m
            kube-system   kubernetes-dashboard-5498ccf677-cgjvq   1/1       Running   0          21m
            kube-system   storage-provisioner                     1/1       Running   0          21m

    1. state of all services

            $ kubectl get services --all-namespaces
            NAMESPACE     NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)         AGE
            default       kubernetes             ClusterIP   10.96.0.1       <none>        443/TCP         24m
            kube-system   kube-dns               ClusterIP   10.96.0.10      <none>        53/UDP,53/TCP   24m
            kube-system   kubernetes-dashboard   NodePort    10.97.164.230   <none>        80:30000/TCP    23m

1. docker

    1. version

            $ docker version
            Client:
             Version:      18.03.1-ce
             API version:  1.37
             Go version:   go1.9.5
             Git commit:   9ee9f40
             Built:        Thu Apr 26 07:13:02 2018
             OS/Arch:      darwin/amd64
             Experimental: false
             Orchestrator: swarm

            Server:
             Engine:
              Version:      18.03.1-ce
              API version:  1.37 (minimum version 1.12)
