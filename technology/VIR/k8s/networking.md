---
title: Kubernetes → networking
description: These tools may be useful if you are debugging connectivity issues, investigating network throughput problems, or exploring Kubernetes to learn how it operates.
published: true
date: 2020-01-18T21:16:28.163Z
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


## traefik



```

microk8s.helm init
microk8s.helm init --override spec.selector.matchLabels.'name'='tiller',spec.selector.matchLabels.'app'='helm' --output yaml | sed 's@apiVersion: extensions/v1beta1@apiVersion: apps/v1@' | microk8s.kubectl apply -f -


cat <<EOF |microk8s.helm install -f -n kube-system traefik stable/traefik
dashboard:
  enabled: "true"
  domain: "traefik-dashboard.2bm.io"
  auth:
    basic:
      admin: $apr1$zjjGWKW4$W2JIcu4m26WzOzzESDF0W/
rbac:
  enabled: "true"
ssl:
  enabled: "true"
  mtls:
    enabled: "true"
    optional: "true"
  generateTLS: "true"
kubernetes:
  ingressEndpoint:
    publishedService: "kube-system/traefik"
metrics:
  prometheus:
    enabled: "true"
EOF


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

