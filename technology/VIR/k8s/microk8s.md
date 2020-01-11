---
title: Kubernetes → microk8s
description: A single package of k8s for 42 flavours of Linux. Made for developers, and great for edge, IoT and appliances.
published: true
date: 2020-01-11T12:37:20.099Z
tags: k8s, vir, microk8s
---

# Header
Your content here


## macOS

```
brew cask install multipass
multipass launch --name k8s-master --mem 8G --disk 40G
multipass list
muttipass shell k8s-master
```


## shell

```
snap info microk8s
sudo snap install microk8s --classic
sudo snap install microk8s --classic --channel=1.16/stable
sudo iptables -P FORWARD ACCEPT


sudo usermod -a -G microk8s $USER


microk8s.kubectl get nodes
microk8s.add-node
  ubuntu@k8s-master:~$ microk8s.add-node
  Join node with: microk8s.join 192.168.64.3:25000/GdnlcwJRAWzigHhodFPmFTIMQHgrSCIZ

microk8s.enable --help
microk8s.enable dns dashboard ingress

microk8s.kubectl proxy --accept-hosts=.* --address=0.0.0.0 &


microk8s.kubectl -n kube-system describe secret $token
```