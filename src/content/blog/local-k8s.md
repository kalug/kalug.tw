---
title: "地端自建 Kubernetes(K8s)小宇宙"
pubDatetime: 2024-12-17
tags:
  - meetup
  
description: "加上 AI 人工智慧與 ML 機器學習的需求， Kubernetes (K8s) 的基礎建設更為重要。
什麼？你想要地端全自建？這是什麼巫術？
在這場演講中，我們會介紹一些常見的 Kubernetes 安裝需要的關鍵元件，分享地端建置甚至到 GPU 主機建置遇到的問題，給大家做一個指引，少走一些彎路"

youtube: g1yuBjMAGog

---

[![hackmd-github-sync-badge](https://hackmd.io/B_74ukSOQniZ2-hcB0Cmzg/badge)](https://hackmd.io/B_74ukSOQniZ2-hcB0Cmzg)




### slide: [Kubernetes 地端自建 v.s. GKE，哪個更適合你？ @Devfest Taipei 2024 | PPT](https://www.slideshare.net/slideshow/kubernetes-v-s-gke-devfest-taipei-2024/273727315)


#### CAP
- Consistency
- Availability
- Partition tolerance

#### kubernetes 10 years
- Kubernetes v1.32 (2024/12)

POD - replica
Service - https://kubernetes.io/docs/concepts/services-networking/service/
- ClusterIP
- NodePort
- LoadBalancer
PVC - PV

Kustomize - https://kustomize.io/
Helm - https://helm.sh/

Local k8s distro
- kubeadm
    - cluster-api https://cluster-api.sigs.k8s.io/
- https://www.rancher.com/
- k3s
- kind - https://kind.sigs.k8s.io/

CNI
- Flannel
- Calico
- Cilium

GPU operator = https://github.com/NVIDIA/gpu-operator



地端離線 airgap install 
 
cri-dockerd - https://mirantis.github.io/cri-dockerd/


### swap

預設建議關掉

Kubernetes 1.28: Beta support for using swap on Linux
https://kubernetes.io/blog/2023/08/24/swap-linux-beta/


### CRI
- dockerd-cri
- cri-o
- containerd
- crictl
    - https://kubernetes.io/docs/tasks/debug/debug-cluster/crictl/


### Netman
https://drive.google.com/file/d/1JnBymxdgXBrRZycvWtOszkl04Gony7n0/view?fbclid=IwZXh0bgNhZW0CMTAAAR2RmfiCzBxlKNjXpu1l8zsuUJF4BAwwI6oqiJgxcbSa4RtfgnPGdadxAWw_aem_jUl1_c8KmdJ6vCh2dFr8LQ
