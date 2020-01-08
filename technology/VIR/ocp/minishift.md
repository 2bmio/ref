---
title: OpenShift → minishift
description: Minishift is a tool that helps you run OKD locally by launching a single-node OKD cluster inside a virtual machine. 
published: true
date: 2020-01-08T23:22:24.644Z
tags: vir, ocp, minishift
---

# Standard
output

```
hift-image-registry" "openshift-router"
Login to server ...
Creating initial project "myproject" ...
Server Information ...
OpenShift server started.

The server is accessible via web console at:
    https://192.168.42.186:8443/console

You are logged in as:
    User:     developer
    Password: <any value>

To login as administrator:
    oc login -u system:admin


-- Exporting of OpenShift images is occuring in background process with pid 12853.
```

### admin access
```
$ minishift addon apply admin-user
$ oc login -u admin
```

### registry playground

```
1. Create ImageStream

    - `oc create -f openjdk-web-basic-s2i.yml  -n openshift`

2. Create ImageStream

    - `oc create is "openjdk-11-rhel7" -n openshift`

minishift ssh

3. Docker PULL

    - `docker pull registry.redhat.io/openjdk/openjdk-11-rhel7:1.1`

4. Docker PULL
$(minishift openshift registry)

docker tag registry.redhat.io/openjdk/openjdk-11-rhel7:1.1 172.30.1.1:5000/openshift/openjdk-11-rhel7:1.1
docker login -u admin -p kiCcJoQiI-iaoffUfstXaDnsxLwm3lGSPNY2fxBcUk4 172.30.1.1:5000
docker push 172.30.1.1:5000/openshift/openjdk-11-rhel7:1.1
```

> Step 1: Install Hypervisor VirtualBox or KVM

> Step 2: Setting up Minishift hypervisor Driver

For Ubuntu / Debian
```
sudo usermod -a -G libvirt $(whoami)
newgrp libvirt || newgrp libvirtd
curl -L https://github.com/dhiltgen/docker-machine-kvm/releases/download/v0.10.0/docker-machine-driver-kvm-ubuntu16.04 -o docker-machine-driver-kvm
sudo mv docker-machine-driver-kvm /usr/local/bin/docker-machine-driver-kvm
sudo chmod +x /usr/local/bin/docker-machine-driver-kvm
```

For Fedora / CentOS
```
sudo usermod -a -G libvirtd $(whoami)
newgrp libvirt
curl -L https://github.com/dhiltgen/docker-machine-kvm/releases/download/v0.10.0/docker-machine-driver-kvm-centos7 -o docker-machine-driver-kvm
sudo mv docker-machine-driver-kvm /usr/local/bin/docker-machine-driver-kvm
sudo chmod +x /usr/local/bin/docker-machine-driver-kvm
```

For Arch Linux / Manjaro
```
sudo usermod -a -G kvm,libvirt $(whoami)
sudo sed -ri 's/.?group\s?=\s?".+"/group = "kvm"/1' /etc/libvirt/qemu.conf
newgrp libvirt
curl -L https://github.com/dhiltgen/docker-machine-kvm/releases/download/v0.10.0/docker-machine-driver-kvm-centos7 -o docker-machine-driver-kvm
sudo mv docker-machine-driver-kvm /usr/local/bin/docker-machine-driver-kvm
chmod +x /usr/local/bin/docker-machine-driver-kvm
```

Start the default KVM network.
```
sudo virsh net-start default
sudo virsh net-autostart default
```

> Step 3: Install Minishift

```
export VER="1.34.1"
curl -L https://github.com/minishift/minishift/releases/download/v$VER/minishift-$VER-linux-amd64.tgz -o minishift-$VER-linux-amd64.tgz
tar xvf minishift-$VER-linux-amd64.tgz
sudo mv minishift-$VER-linux-amd64/minishift /usr/local/bin 
$ minishift version
minishift v1.34.1+c2ff9cb
```

> essntials commands


```
$ minishift start --vm-driver virtualbox
$ minishift config set vm-driver virtualbox
$ minishift console --url
$ minishift console
  Username: system
  Password: admin
```


```

```


```

```


```

```





