---
title: Kubernetes → networking
description: These tools may be useful if you are debugging connectivity issues, investigating network throughput problems, or exploring Kubernetes to learn how it operates.
published: true
date: 2020-01-19T11:27:19.698Z
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

microk8s.helm install -f traefik-values.yaml -n kube-system traefik stable/traefik --version 1.85.1
microk8s.helm delete -n kube-system traefik


vi helm-traefik-values.yaml
###################
dashboard:
  enabled: true
  domain: traefik.example.com
  auth:
    basic:
      admin: $apr1$zjjGWKW4$W2JIcu4m26WzOzzESDF0W/
rbac:
  enabled: true
kubernetes:
  ingressEndpoint:
    publishedService: kube-system/traefik
metrics:
  prometheus:
    enabled: true
ssl:
  enabled: true
  generateTLS: true
# # no-working-yet
  # mtls:
  #   enabled: true
  #   optional: true

###################

vi traefik-ds.yaml

---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: traefik-ingress-controller
rules:
  - apiGroups:
      - ""
    resources:
      - pods
      - services
      - endpoints
      - secrets
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - extensions
    resources:
      - ingresses
    verbs:
      - get
      - list
      - watch
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: traefik-ingress-controller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: traefik-ingress-controller
subjects:
- kind: ServiceAccount
  name: traefik-ingress-controller
  namespace: kube-system
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: traefik-ingress-controller
  namespace: kube-system
---
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: kube-system
  name: traefik-conf
data:
  traefik.toml: |
    # traefik.toml
    logLevel = "DEBUG"
 
    [traefikLog]
      filePath = "log/traefik.log"
      format   = "json"
 
    [accessLog]
      filePath = "log/access.log"
      format = "json"
    
    defaultEntryPoints = ["http"]
    
    [entryPoints]
        [entryPoints.http]
        users =  ['admin:$apr1$ZywpxeoS$6U80kYPG116slxBceEsVz0']
        address = ":9090"
    
    [web]
    address = ":8095"
    
    [backends]
      [backends.backend]
         [backends.backend.LoadBalancer]
           method = "wrr"
         [backends.backend.servers.server1]
         url = ":8080"
         weight = 1
    
    [frontends]
      [frontends.frontend1]
      backend = "backend"
        [frontends.frontend1.routes.test_1]
        rule = "Host:dashboard-traefik.domain.com"
---
kind: DaemonSet
apiVersion: apps/v1
metadata:
  name: traefik-ingress-controller
  namespace: kube-system
  labels:
    k8s-app: traefik-ingress-lb
spec:
  selector:
    matchLabels:
      name: traefik-ingress-lb
  template:
    metadata:
      labels:
        k8s-app: traefik-ingress-lb
        name: traefik-ingress-lb
    spec:
      serviceAccountName: traefik-ingress-controller
      terminationGracePeriodSeconds: 60
      volumes:
      - name: config
        configMap:
          name: traefik-conf
      # Enable this only if using static wildcard cert
      # stored in a k8s Secret instead of LetsEncrypt
      #- name: ssl
      #  secret:
      #    secretName: traefik-cert
      containers:
      - image: traefik
        name: traefik-ingress-lb
        resources:
          limits:
            cpu: 200m
            memory: 30Mi
          requests:
            cpu: 100m
            memory: 20Mi
        volumeMounts:
        - mountPath: "/config"
          name: "config"
        ports:
        - name: http
          containerPort: 80
        - name: admin
          containerPort: 8080
        args:
        - --api
        - --kubernetes
        - --logLevel=INFO
        - --web
        - --kubernetes
        - --configfile=/config/traefik.toml
---
kind: Service
apiVersion: v1
metadata:
  name: traefik-ingress-service
  namespace: kube-system
spec:
  selector:
    k8s-app: traefik-ingress-lb
  ports:
    - protocol: TCP
      port: 80
      name: web
    - protocol: TCP
      port: 8080
      name: admin
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

