---
title: Kubernetes → home
description: Kubernetes is a portable, extensible, open-source platform for managing containerized workloads and services.
published: true
date: 2020-02-04T11:37:13.990Z
tags: k8s, vir
---

## CheatSheet

```
kubectl get services                # List all services 
kubectl get pods                    # List all pods
kubectl get nodes -w                # Watch nodes continuously
kubectl version                     # Get version information
kubectl cluster-info                # Get cluster information
kubectl config view                 # Get the configuration
kubectl describe node <node>        # Output information about a node
kubectl get pods                         # List the current pods
kubectl describe pod <name>              # Describe pod <name>
kubectl get rc                           # List the replication controllers
kubectl get rc --namespace="<namespace>" # List the replication controllers in <namespace>
kubectl describe rc <name>               # Describe replication controller <name>
kubectl get svc                          # List the services
kubectl describe svc <name>              # Describe service <name>

kubectl run <name> --image=<image-name>                             # Launch a pod called <name> 
                                                                    # using image <image-name> 
kubectl create -f <manifest.yaml>                                   # Create a service described 
                                                                    # in <manifest.yaml>
kubectl scale --replicas=<count> rc <name>                          # Scale replication controller 
                                                                    # <name> to <count> instances
kubectl expose rc <name> --port=<external> --target-port=<internal> # Map port <external> to 
                                                                    # port <internal> on replication 
                                                                    # controller <name>
kubectl delete pod <name>                                         # Delete pod <name>
kubectl delete rc <name>                                          # Delete replication controller <name>
kubectl delete svc <name>                                         # Delete service <name>
kubectl drain <n> --delete-local-data --force --ignore-daemonsets # Stop all pods on <n>
kubectl delete node <name>                                        # Remove <node> from the cluster
kubectl exec <service> <command> [-c <$container>] # execute <command> on <service>, optionally 
                                                   # selecting container <$container>
kubectl logs -f <name> [-c <$container>]           # Get logs from service <name>, optionally
                                                   # selecting container <$container>
watch -n 2 cat /var/log/kublet.log                 # Watch the Kublet logs
kubectl top node                                   # Show metrics for nodes
kubectl top pod                                    # Show metrics for pods
kubeadm init                                              # Initialize your master node
kubeadm join --token <token> <master-ip>:<master-port>    # Join a node to your Kubernetes cluster
kubectl create namespace <namespace>                      # Create namespace <name>
kubectl taint nodes --all node-role.kubernetes.io/master- # Allow Kubernetes master nodes to run pods
kubeadm reset                                             # Reset current state
kubectl get secrets                                       # List all secrets

```


## ninja


```
# -------- 0		→ Accessing Clusters
kubectl config view


# -------- 1		→ Kubectl Autocomplete
# bash
source <(kubectl completion bash) 
echo "source <(kubectl completion bash)" >> ~/.bashrc 
alias k=kubectl
complete -F __start_kubectl k

# zsh
source <(kubectl completion zsh)  # setup autocomplete in zsh into the current shell
echo "if [ $commands[kubectl] ]; then source <(kubectl completion zsh); fi" >> ~/.zshrc # add autocomplete permanently to your zsh shell

## kubectl get ns <tab>

# -------- 2		→ Kubectl apply/delete from file
kubectl apply -f manifest.yaml
kubectl delete -f manifest.yaml

# -------- 3		→ Kubectl Run
kubectl run -it alpine --image=alpine -- sh

# -------- 4		→ Get pods con labels
kubectl get pods -l run=nginx --all-namespaces

# -------- 5		→ Get pods (o cualquier cosa) diferente formato
kubectl get pods -o=name
kubectl get pods -o=wide
kubectl get pods -o=yaml
kubectl get pods -o=json

# -------- 6		→ Get pods para hacer un manifest
kubectl -n nginx1 get pod nginx-6fd8984946-8m648 -o yaml --export
kubectl -n rabbitmq get pods
oc get cm/logstash --export -o yaml -n rabbitmq >cm-logstash.yml



# -------- 7		→ Get the version label of all pods with label app=cassandra
kubectl get pods -l run=nginx --all-namespaces -o \
  jsonpath='{.items[*].spec.containers[*].image}'

# -------- 8		→ Estadisticas de un o varios nodos
kubectl top node <nombre-del-nodo>

# -------- 9		→ Traerte el estado de los componentes
kubectl get componentstatuses

# -------- 10		→ Traerte los recursos disponibles
kubectl api-resources

```


# CONCEPTS
## Containers
## Workloads
### Controllers → estados declarativos
#### ReplicaSet → new way
> Es un OBJETO Podemos tener una app que no tenga downtime → el famos estamos actualizando el servicio → cuando las app se siguen actualizando
Garantiza que existen un número predefinido de pods funcionando al mismo tiempo.
Se indica el número de réplicas con el valor "replicas: 2"
#### ReplicationController → old way
> Asegura tolerancia a fallos sobre un conjuto de PODs: es importante que el POD gestionado por el RC sea STATELESS, no es útil con base de datos ni sesiones.

#### ReplicaSet/ReplicationController

Replica Set is the next generation of Replication Controller. Replication controller is kinda imperative, but replica sets try to be as declarative as possible.

1.The main difference between a Replica Set and a Replication Controller right now is the selector support.

```
+--------------------------------------------------+-----------------------------------------------------+
|                   Replica Set                    |               Replication Controller                |
+--------------------------------------------------+-----------------------------------------------------+
| Replica Set supports the new set-based selector. | Replication Controller only supports equality-based |
| This gives more flexibility. for eg:             | selector. for eg:                                   |
|          environment in (production, qa)         |             environment = production                |
|  This selects all resources with key equal to    | This selects all resources with key equal to        |
|  environment and value equal to production or qa | environment and value equal to production           |
+--------------------------------------------------+-----------------------------------------------------+

```

2.The second thing is the updating the pods.

```
+-------------------------------------------------------+-----------------------------------------------+
|                      Replica Set                      |            Replication Controller             |
+-------------------------------------------------------+-----------------------------------------------+
| rollout command is used for updating the replica set. | rolling-update command is used for updating   |
| Even though replica set can be used independently,    | the replication controller. This replaces the |
| it is best used along with deployments which          | specified replication controller with a new   |
| makes them declarative.                               | replication controller by updating one pod    |
|                                                       | at a time to use the new PodTemplate.         |
+-------------------------------------------------------+-----------------------------------------------+

```

These are the two things that differentiates RS and RC. Deployments with RS is widely used as it is more declarative.


#### Deployments
> Es un OBJETO de una abstracción un poco mayor a un ReplicaSet el cual tiene mayor rangos de acción en forma declarativa, esto quere decir por ejemplo que permite rolling updates, rolleback, cleanup, pod scaling, replica management.
A diferencia de un ReplicaSet, un Deployment incorpora RevissionHistoryLimit (rolleback) y strategy (RollingUpdate o Recrete)
Se pueden usar annotations para automatizaciones.
#### StatefulSets > petSets
> El concepto de mascota → Gestiona el despliegue y escalado de un conjunto de Pods , y garantiza el orden y unicidad de dichos Pods. StatefulSet mantiene una identidad asociada a sus Pods. Estos pods se crean a partir de la misma especificación, pero no pueden intercambiarse; cada uno tiene su propio identificador persistente que mantiene a lo largo de cualquier re-programación.

> Un StatefulSet opera bajo el mismo patrón que cualquier otro controlador. Se define el estado deseado en un objeto StatefulSet, y el controlador del StatefulSet efectúa las actualizaciones que sean necesarias para alcanzarlo a partir del estado actual.

> Hay momentos en los que necesitamos que los pod sean permantes y tenerlos localizados bajo un mismo nombre por ejemplo cuando usamos base de datos.
Para automatizar este tipo de despligue a a menudo en el mismo pod se suele poner un script que auto-configura este servicio, para ello se usa un configMap → sideCard

#### DaemonSet
> Son Deployments que se ejectan en todos los nodos

> Un DaemonSet garantiza que todos (o algunos) de los nodos ejecuten una copia de un Pod. Conforme se añade más nodos al clúster, nuevos Pods son añadidos a los mismos. Conforme se elimina nodos del clúster, dichos Pods se destruyen. Al eliminar un DaemonSet se limpian todos los Pods que han sido creados.

> Algunos casos de uso típicos de un DaemonSet son:
Ejecutar un proceso de almacenamiento en el clúster, como glusterd, ceph, en cada nodo.
Ejecutar un proceso de recolección de logs en cada nodo, como fluentd o logstash.
Ejecutar un proceso de monitorización de nodos en cada nodo, como Prometheus Node Exporter, Sysdig Agent, collectd, Dynatrace OneAgent, AppDynamics Agent, Datadog agent, New Relic agent, Ganglia gmond o un agente de Instana.


#### Garbage Collection
#### TTL Controller for Finished Resources
#### Jobs - Run to Completion
#### CronJob










