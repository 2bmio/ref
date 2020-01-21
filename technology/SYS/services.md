---
title: System → services
description: 
published: true
date: 2020-01-21T16:58:45.157Z
tags: sys,  services
---

# Daemons
also known as background processes

## systemd

> path of the journald configuration file `/etc/systemd/journald.conf`
> ```
>  SystemMaxUse=1G
>  MaxFileSec=7day
> ```


#### restart service


`systemctl restart systemd-journald`



> e.g.: to remove archived journal files until the disk space they use falls below 100M `SystemMaxUse=100M`

> e.g.: to make all journal files contain no data older than 5 days `MaxFileSec=5day`


### references
```
# journalctl --disk-usage
# journalctl --rotate
# journalctl --vacuum-time=1s
# journalctl --rotate --vacuum-size=500M
```


---------------------------


```
01 - Zipkin. Cache para microservicios
02 - RabbitMQ. Cola de mensajería para logs de microservicios  
03 - Icinga. Monitorización de S.O. 1 agente por máquina
04 - InfluxDB. Base de datos para métricas de servicios. Es donde se guardan las métricas de icinga y collectd 
05 - MySQL. 1 instancia en liic01 y otra en libs01. Lo usan rundeck, nexus, icinga ...
06 - Java. Runtime para microservicios
07 - CollectD. Permite la recolección de métricas a modo de monitorización para S.O. y otros servicios. 1 agente por máquina.
08 - Grafana. Muestra dashboards de métricas. Recoge los datos de influxdb
09 - Elasticsearch. Se usa para almacenar logs y transacciones de microservicios 
10 - Rundeck. Portal para gestionar scripts y otros procesos de automatización sobre PaaS 
11 - Ansible. Despliegue de OpenShift. Automatización de configuración en máquinas virtuales.
12 - Jenkins. Orquestador de ciclo de vida de un microservicio. 
13 - Sonarqube. Validación de código estático de microservicios.
14 - Nexus. Caché de artefactos para compilar microservicios.
15 - Docker. Motor de contenedores.
16 - Maven. 
17 - Lenguaje de progamación.
18 - OpenShift. Orquestador de contenedores.
19 - Logstash. Log appender 
20 - Vault. Gestor de secretos y tokens 
21 - Consul. Service discovering, configuration discovering 
22 - Heketi. Orquestador de provisión dinámica de volumenes sobre glusterfs 
23 - Kibana. Muestra dashboards de elasticsearch 
24 - GlusterFS. Cluster de almacenamiento POSIX distribuido.
25 - CICD. Metodología de trabajo para mejorar el ciclo de vida de un microservicio. 
26 - WSO2. Api Manager.

```




