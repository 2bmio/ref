---
title: OpenShift → laboratories
description: testing and playground 
published: true
date: 2020-01-13T09:50:20.128Z
tags: vir, ocp, labs
---

# exploshift

> Version
> OpenShift Master: v3.9.71 
> Kubernetes Master: v1.9.1+a0ce1bc657 
> OpenShift Web Console: v3.9.71 

```
# direcct connect
t es
```


## cleanUp

```
# oc get nodes
NAME           STATUS    ROLES     AGE       VERSION
vassvm-10001   Ready     master    304d      v1.9.1+a0ce1bc657
vassvm-10002   Ready     <none>    304d      v1.9.1+a0ce1bc657
vassvm-10003   Ready     <none>    304d      v1.9.1+a0ce1bc657
vassvm-10004   Ready     compute   304d      v1.9.1+a0ce1bc657
vassvm-10005   Ready     compute   304d      v1.9.1+a0ce1bc657


oc get pod -o wide --all-namespaces | grep vassvm-10002


```



# proshift

> Version
> OpenShift Master: v3.11.141 
> Kubernetes Master: v1.11.0+d4cacc0 
> OpenShift Web Console: v3.11.141 


```
# direcct connect
t ps
```


# ultrashift
OpenShift Version: 4.2.2
Kubernetes Version: v1.14.6+868bc38
Channel: stable-4.2

```
# direcct connect
t us
```

# API manager

```
connect to address 192.168.XXX.XXX and port 9444: Connection refused
HTTP CRITICAL - Unable to open TCP socket
```






