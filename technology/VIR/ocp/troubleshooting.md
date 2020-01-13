---
title: OpenShift → troubleshooting
description: 
published: true
date: 2020-01-13T11:35:29.956Z
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