---
title: OpenShift → adm
description: administrator CLI operations and their syntax.
published: true
date: 2020-01-17T12:08:44.817Z
tags: vir, ocp
---


# get cluster info

```
oc adm top node --heapster-namespace=openshift-infra --heapster-scheme=https
oc adm top pods --heapster-namespace='openshift-infra' --heapster-scheme="https" --all-namespaces > consumoPods.txt

```

# Troubleshooting


## Issues
### Error response from daemon: Get : x509: certificate is valid for ...

```
# standard way
oc adm prune images --confirm

# using flags, in this case, specific:
## --registry-url
## --force-insecure

oc adm prune images --registry-url=https://docker-registry.default.svc.cluster.local:5000 --confirm  --force-insecure
```

