---
title: OpenShift → minishift
description: Minishift is a tool that helps you run OKD locally by launching a single-node OKD cluster inside a virtual machine. 
published: true
date: 2020-01-08T22:38:00.870Z
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

