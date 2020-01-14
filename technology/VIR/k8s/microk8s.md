---
title: Kubernetes → microk8s
description: A single package of k8s for 42 flavours of Linux. Made for developers, and great for edge, IoT and appliances.
published: true
date: 2020-01-14T06:30:16.491Z
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

## fedora

```
sudo dnf install snapd
sudo ln -s /var/lib/snapd/snap /snap
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

microk8s.status
microk8s.enable --help
microk8s.enable dns dashboard ingress

## List of the most important addons

*   dns: Deploy DNS. This addon may be required by others, thus we recommend you always enable it.
*   dashboard: Deploy kubernetes dashboard as well as grafana and influxdb.
*   storage: Create a default storage class. This storage class makes use of the hostpath\-provisioner pointing to a directory on the host.
*   ingress: Create an ingress controller.
*   gpu: Expose GPU(s) to MicroK8s by enabling the nvidia\-docker runtime and nvidia\-device\-plugin\-daemonset. Requires NVIDIA drivers to be already installed on the host system.
*   istio: Deploy the core Istio services. You can use the `microk8s.istioctl` command to manage your deployments.
*   registry: Deploy a docker private registry and expose it on localhost:32000. The storage addon will be enabled as part of this addon.

microk8s.kubectl -n kube-system get secret

microk8s.kubectl -n kube-system describe secret kubernetes-dashboard-token-XXXXX


microk8s.kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443 --address 0.0.0.0

```