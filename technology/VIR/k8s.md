---
title: Kubernetes → home
description: Kubernetes is a portable, extensible, open-source platform for managing containerized workloads and services.
published: true
date: 2020-02-04T09:57:59.370Z
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
#### ReplicaSet
> Es un OBJETO Podemos tener una app que no tenga downtime → el famos estamos actualizando el servicio → cuando las app se siguen actualizando
Garantiza que existen un número predefinido de pods funcionando al mismo tiempo.
Se indica el número de réplicas con el valor "replicas: 2"
#### ReplicationContreller
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
#### Garbage Collection
#### TTL Controller for Finished Resources
#### Jobs - Run to Completion
#### CronJob










