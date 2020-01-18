---
title: Kubernetes → networking
description: These tools may be useful if you are debugging connectivity issues, investigating network throughput problems, or exploring Kubernetes to learn how it operates.
published: true
date: 2020-01-18T19:27:38.963Z
tags: k8s, vir, networking
---

## metalLB

```
https://metallb.universe.tf/installation/

kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.8.3/manifests/metallb.yaml

# get the range IP of 10.156.103.XXX-10.156.103.XXX
❯ multipass ls
Name                    State             IPv4             Image
k8s-app1                Running           10.156.103.230   Ubuntu 18.04 LTS
k8s-app2                Running           10.156.103.48    Ubuntu 18.04 LTS
k8s-master              Running           10.156.103.63    Ubuntu 18.04 LTS


vi metallb-configmap.yaml

apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 10.156.103.100-10.156.103.110

microk8s.kubectl apply -f metallb-configmap.yaml

microk8s.kubectl edit service/kubernetes-dashboard -n kube-system

  type: LoadBalancer

```

## addons


# debuggin

conntrack

Khan - Pod Connection Tracking Metrics Exporter
https://github.com/att-cloudnative-labs/khan
https://www.digitalocean.com/community/tutorials/how-to-inspect-kubernetes-networking

conntrack is installed via toolbox on CoreOS Container Linux:


```
# /usr/bin/toolbox
# dnf -y install conntrack
# conntrack -h
```

