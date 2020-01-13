---
title: OpenShift → troubleshooting
description: 
published: true
date: 2020-01-13T13:25:33.906Z
tags: vir, ocp, troubleshooting
---

# Header
Your content here

### issue → Metricas entorno test

### changging dc/logstash

```
NAME          REVISION   DESIRED   CURRENT   TRIGGERED BY
dc/logstash   48         0         0         image(logstash:6.1.1)
NAME          DOCKER REPO                                          
TAGS                              UPDATED
is/logstash   docker-registry.default.svc:5000/rabbitmq/logstash   6.1.1                             9 months ago
```

### docker-registry route and service

```
# con este comando obtendremos un código 200 → indica que el servicio responde y toda la comunicación con el registry
curl -v https://docker-registry-default.apps.exploshift.vass.es/healthz

# con este comando comprobamos que el registry es capaz de reconocernos y dejarnos hacer login
docker login -u admin -p <TOKEN> docker-registry-default.apps.exploshift.vass.es:443

# toda la información la he obtenido de aquí:
https://docs.openshift.com/container-platform/3.3/install_config/registry/securing_and_exposing_registry.html


```


### backup and resore project

```
Backing up a project

# 1 List all the relevant data to back up:
oc get all

# 2 Export the project objects to a .yaml
oc get -o yaml --export all > project.yaml

# 3 Export the project’s role bindings, secrets, service accounts, and persistent volume claims

$ for object in rolebindings serviceaccounts secrets imagestreamtags cm egressnetworkpolicies rolebindingrestrictions limitranges resourcequotas pvc templates cronjobs statefulsets hpa deployments replicasets poddisruptionbudget endpoints
do
  oc get -o yaml --export $object > $object.yaml
done

# 4 To list all the namespaced objects:
oc api-resources --namespaced=true -o name
```
