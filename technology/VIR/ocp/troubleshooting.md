---
title: OpenShift → troubleshooting
description: 
published: true
date: 2020-01-14T11:46:28.965Z
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
https://docs.openshift.com/dedicated/3/admin_guide/assembly_backing-up-restoring-project-application.html

1 List all the relevant data to back up:
oc get projects -o wide
oc get all -n <namespace>

2 Export the project objects to a .yaml
oc get -o yaml --export all -n <namespace> > project.yaml


3 Export the project’s role bindings, secrets, service accounts, and persistent volume claims

for object in rolebindings serviceaccounts secrets imagestreamtags cm egressnetworkpolicies rolebindingrestrictions limitranges resourcequotas pvc templates cronjobs statefulsets hpa deployments replicasets poddisruptionbudget endpoints
do
  oc get -o yaml --export $object -n <namespace> > $object.yaml
done

4 To list all the namespaced objects:
oc api-resources --namespaced=true -o name

```


### force delete OpenShift project


```
#!/bin/sh
#
# A script to delete namespace(s) stuck at "Terminating" state, that won't get deleted, within the OpenShift cluster. 
# You'll see an error message like this below, when you try "oc delete project <namespace>"
#
# Error from server (Conflict): Operation cannot be fulfilled on namespaces "<namespace>": The system is ensuring all content is removed from this namespace. Upon completion, this namespace will automatically be purged by the system.
#
####### </ Jeffery Bagirimvano >

while getopts n:u:t:h--help: option; do
  case "${option}" in
    n) NS_LIST=${OPTARG};;
    u) REST_API_URL=${OPTARG};;
    t) TOKEN=${OPTARG};;
    h|--help)
      echo "
Options:
  -n: List of namespaces to delete
  -u: The OpenShift server's REST API URL. Default: 'oc whoami --show-server'
  -t: Token to use. Default: 'oc whoami -t'
  
Examples:
  # To get help:
  force-delete-openshift-project -h
  
  # To delete 'test123' namespace:
  force-delete-openshift-project -n test123
  
  # To delete more than one namespaces, 'test123 alpha beta' namespaces:
  force-delete-openshift-project -n 'test123 alpha beta'

  # To use a different url:
  force-delete-openshift-project -n test123 -u https://console.example.com:8443
  
  # To provide a token to use:
  force-delete-openshift-project -n test123 -t _gevQ0rnvXfMJzkFy1NPh9Kf4jYOk1qhNT6wb4
  force-delete-openshift-project -n test123 -u https://console.example.com:8443 -t _gevQ0rnvXfMJzkFy1NPh9Kf4jYOk1qhNT6wb4
  "
      exit 0
      ;;
  esac
done

NAMESPACE_LIST="${NS_LIST}"
OC_TOKEN="${TOKEN:-$(oc whoami -t)}"
OC_REST_API_URL="${REST_API_URL:-$(oc whoami --show-server)}"

if [ ! -z "$NAMESPACE_LIST" ]; then
  echo
  for namespace in ${NAMESPACE_LIST}; do
    echo "Working on deleting ${namespace}"
    oc get ns ${namespace} -o json > delete-${namespace}-project.json
    sed -i '/"kubernetes"/d' delete-${namespace}-project.json
    curl --silent --insecure -H "Content-Type: application/json" -H "Authorization: Bearer ${OC_TOKEN}" -X PUT --data-binary @delete-${namespace}-project.json ${OC_REST_API_URL}/api/v1/namespaces/${namespace}/finalize
    rm -f delete-${namespace}-project.json
  done
  echo

else
  echo
  echo "It looks like you haven't set the namespace(s)/project(s) to delete"
  echo "Check Options by using -h or --help"
  echo
fi

```

