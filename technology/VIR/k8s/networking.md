---
title: Kubernetes → networking
description: These tools may be useful if you are debugging connectivity issues, investigating network throughput problems, or exploring Kubernetes to learn how it operates.
published: true
date: 2019-12-27T11:31:34.963Z
tags: k8s, vir, networking
---

# general knoled

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

