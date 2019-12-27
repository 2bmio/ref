---
title: OpenShift → networking
description: 
published: true
date: 2019-12-27T12:35:15.417Z
tags: vir, ocp, networking
---

# tools
retrive network info 

```
  oc get hostsubnets
  oc get clusternetwork
```


### get routes

```
## get all cluster routes
oc get route --all-namespaces

## grep some special route
oc get route --all-namespaces | grep 'sub.domain.tls'
```