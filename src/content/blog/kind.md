---
title: "kind: kubernetes in docker"
pubDatetime: 2025-01-16T19:30:00+08
tags:
  - meetup
  - kubernetes
  - kind
  - podman
  
description: "let's run kind in podman"
youtube: b-b9Nahro9w
luma: dt8l2tnm
googlemap: omXnciLVyBBzHcLXA
meetupLocation: Second space 捷運中央公園站 三號出口 R9 Exit 3 

---

[![hackmd-github-sync-badge](https://hackmd.io/o6XKOGGyREyfbxsSDNveJg/badge)](https://hackmd.io/o6XKOGGyREyfbxsSDNveJg)


本次活動我們會在 second space 與 線上同時舉行
上次 Johnny 幫我們介紹 本地端 如何建 kubernetes,
而 單純 本地測試 還有一個更輕量的選擇 kind
Shawn 打算介紹如何使用 用 podman 使用 kind, 以及 podman 一些好用的功能如:

- podman kube
- daemonless / quadlet
- podmansh


---

# kind

### kubernetes in docker (or podman)


:::info
2025/01 KaLUG.tw meetup / Shawn
👉[youtube](https://www.youtube.com/watch?v=b-b9Nahro9w) / [github demo code](https://github.com/kalug/kind-demo)👈 
:::

:::danger
⚡ Don't use for PRODUTION env ⚡
:::

----

# Outline

* what's kind?
* other choices
* inside kind
  - docker in docker
  - LoadBalancers - 
cloud-provider-kind


:::info
👍 using kind for dev or testing
:::

---

## What's kind

- Kubernetes in Docker
https://kind.sigs.k8s.io/

- Go packages implementing cluster creation, image build, etc.
- A command line interface (kind) built on these packages.
- Docker image(s) written to run systemd, Kubernetes, etc.
- kubetest (WIP)

----

### Deep Dive: Kind
Nov 22, 2019
{%youtube tT-GiZAr6eQ%}

----

### Deep Dive: Kind

* test kubernetes
* E2E testing
* 7:30 Networking Deep Dive (kindnet)


----

### Testing your K8s apps with KIND

May 24, 2019
{%youtube 8KtmevMFfxA %}

----

### Quick start

https://kind.sigs.k8s.io/docs/user/quick-start/

```
# fetch release binary
curl -Lo ~/bin/kind https://kind.sigs.k8s.io/dl/v0.26.0/kind-linux-amd64 && chmod +x ~/bin/kind
```

``` 
$ kind create cluster
```

```
$ kind get clusters
kind
```

```
$ kubectl get nodes -o wide
NAME                 STATUS   ROLES           AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                         KERNEL-VERSION                                        CONTAINER-RUNTIME
kind-control-plane   Ready    control-plane   52s   v1.32.0   10.89.0.2     <none>        Debian GNU/Linux 12 (bookworm)   6.13.0-0.rc1.20241206gitb8f52214c61a.19.fc42.x86_64   containerd://1.7.24
```

----

### What is a kubernetes

![k](https://kubernetes.io/images/docs/components-of-kubernetes.svg)

----

### What is a kind cluster? - kind nodes

```
$ podman ps

CONTAINER ID  IMAGE                                                                                           COMMAND     CREATED        STATUS        PORTS                      NAMES
13b1bfcafa48  docker.io/kindest/node@sha256:c48c62eac5da28cdadcf560d1d8616cfa6783b58f0d94cf63ad1bf49600cb027              3 minutes ago  Up 3 minutes  127.0.0.1:37513->6443/tcp  kind-control-plane
```

```
$  podman exec -it kind-control-plane bash
root@kind-control-plane:/# systemctl status kubelet.slice
● kubelet.slice - slice used to run Kubernetes / Kubelet
     Loaded: loaded (/etc/systemd/system/kubelet.slice; static)
     Active: active since Wed 2024-12-25 14:25:51 UTC; 5min ago
      Tasks: 174
     Memory: 460.4M
        CPU: 26.736s
     CGroup: /kubelet.slice
             ├─kubelet-kubepods.slice
...
```

----

### containers in kind node

```
ctr  -n k8s.io c ls
CONTAINER                                                           IMAGE                                                          RUNTIME
0cc5dbdf2b34ff037bae5693ee6e64d3c9cb9f8ae32ab153d0e6cdc8a12eebd2    registry.k8s.io/pause:3.10                                     io.containerd.runc.v2
2584e9ca5a3c9725b76539ee7e736adee1c287ccd8d55031f5afdf9fa6c3390d    registry.k8s.io/kube-apiserver-amd64:v1.32.0                   io.containerd.runc.v2
37104ddf07cf3ce0bc8b56659d7a17304f017b6d4ce652301a91e5324b9cf82a    registry.k8s.io/kube-proxy-amd64:v1.32.0                       io.containerd.runc.v2
3f8dfdd4eac682e21a082eb89f6ff98f37cf666e1cc93be7b1bbdfd3b0dff320    registry.k8s.io/kube-scheduler-amd64:v1.32.0                   io.containerd.runc.v2

...
```

---

# Others?


Kind < k3d < minikube

----

|          | features      | vm       | vendor | LB |
| -------- | --------      | -------- | ------ | --- |
| kind     | lite          | no       | sig-testing | cloud-provider-kind |
| minikube | full features | yes      | sig-cluster-lifecycle | mikikube tunnel |
| kubeadm  | -             | -        | Kubernetes | - |
| k3d      | k3s           | -        | k3s |  via Ingress (recommended) |
| microk8s | snap          | yes      | canonical | MetalLB |
| [capid](https://cluster-api.sigs.k8s.io/) | cluster-api | no |  sig-cluster-api | |


----

## tips:

### kubie
A more powerful alternative to kubectx and kubens
https://github.com/sbstp/kubie

- Shell prompt modification
- Split configuration files
- kubie exec

### known-issues
- https://kind.sigs.k8s.io/docs/user/known-issues/
- like max_user_instances


---

## Inside kind

- privileged mode
  - Docker in Docker
  - systemd in Docker
- load

---

### Docker in Docker
#### container inside container

- \-\-privileged
- Linux Security Modules
    - AppArmor or SELinux

----

#### privileged mode
https://learn.snyk.io/lesson/container-runs-in-privileged-mode/



----

### systemd in Docker

```
from debian:11
run apt -y update
run apt -y install systemd

# first tell systemd that it is in docker (it will check for the container env)
# https://systemd.io/CONTAINER_INTERFACE/
ENV container=docker

# systemd exits on SIGRTMIN+3, not SIGTERM (which re-executes it)
# https://bugzilla.redhat.com/show_bug.cgi?id=1201657
STOPSIGNAL SIGRTMIN+3
# NOTE: this is *only* for documentation, the entrypoint is overridden later
RUN passwd -d root
ENTRYPOINT [ "/lib/systemd/systemd" ]
```

----

## more deeper

- kubelet - https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/
- kubeadm - 
- kubelet plugin
- nvkind - https://github.com/NVIDIA/nvkind/
- cri-o in kind

----

### kubeadm

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/




----

cri-o in kind
https://github.com/cri-o/cri-o/blob/main/tutorials/crio-in-kind.md

---

### kindnet

CNI - https://github.com/containernetworking/cni

- cni/net.d/10-kindnet.conflist


---

### LoadBalancers - cloud-provider-kind

https://kccnceu2024.sched.com/event/1YhhY

https://github.com/kubernetes-sigs/cloud-provider-kind

{%youtube U6_-y24rJnI %}


----

### cloud-provider-kind - one service one kindccm

```
podman ps

a0a8b3c02bab  docker.io/envoyproxy/envoy:v1.30.1                                                              bash -c echo -en ...  About an hour ago  Up About an hour  0.0.0.0:40617->10000/tcp   kindccm-KDNZI53UFUSNY4WSWHFWVBBUZDBQ3EQ5KR66Y5RJ

```

----

* kubectl port-forward service/doc-controller 8080：8080

https://kccnceu2024.sched.com/event/1YhhY/keep-calm-and-load-balance-on-kind-antonio-ojea-benjamin-elder-google


----

## Other tips
- Local Registry
- kubie


----

## nvkind

- https://github.com/NVIDIA/nvkind/
- https://github.com/NVIDIA/k8s-device-plugin/
- https://www.youtube.com/watch?v=jnHlwZKJiL4

- docker only

{%youtube jnHlwZKJiL4 %}


