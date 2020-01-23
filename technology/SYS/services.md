---
title: System → services
description: 
published: true
date: 2020-01-23T09:44:01.515Z
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

09.00 min
---------------------------


### ¿como observar?
```
Observabilidad:

muestran estados:
  08'- Icinga Monitor
  08 - Grafana. Muestra dashboards de métricas. Recoge los datos de influxdb
        - permite historicos
        - lo usamos para enviar alertas
  02 - RabbitMQ. Cola de mensajería para logs de microservicios  
  23 - Kibana. Muestra dashboards de elasticsearch

recogen estados:
  03 - Icinga. Cliente Monitorización de S.O. 1 agente por máquina
        - tiene un agente en cada máquina
  07 - CollectD. Permite la recolección de métricas a modo de monitorización para S.O. y otros servicios. 1 agente por máquina.
        - tiene un agente en cada máquina
        -	para comprobar kibana
        - para comprobar el status de elastik search
        - por defecto tiene unas metricas que recoge valores conocidos del sistema operativo o scripts
        - manda trazas a influx nosotros lo tenemos puesto en influx
        - lo que envía a influx es → el timestamp (del momento de la ejecución) + una clave + un valor 
	19 - Logstash. Log appender
almacenan estados:
	cluster structure:
		04 - InfluxDB. Base de datos para métricas de servicios. Es donde se guardan las métricas de icinga y collectd
		05 - MySQL. 1 instancia en liic01 y otra en libs01. Lo usan rundeck, nexus, icinga ...
	microservicios:
		09 - Elasticsearch. Es una base de datos: Se usa para almacenar logs y transacciones de microservicios








```

### ¿como operar?
```
Operabilidad:

operaciones:
  18 - OpenShift. Orquestador de contenedores.
    15 - Docker. Motor de contenedores.
    20 - Vault. Gestor de secretos y tokens 
    21 - Consul. Service discovering, configuration discovering 

mantenimiento
  10 - Rundeck. Portal para gestionar scripts y otros procesos de automatización sobre PaaS 
    11 - Ansible. Despliegue de OpenShift. Automatización de configuración en máquinas virtuales.

cicd
  12 - Jenkins. Orquestador de ciclo de vida de un microservicio. 
      01 - Zipkin. Cache para microservicios
      13 - Sonarqube. Validación de código estático de microservicios.
      14 - Nexus. Caché de artefactos para compilar microservicios.
      16 - Maven.

almacenamiento
  24 - GlusterFS. Cluster de almacenamiento POSIX distribuido.
  22 - Heketi. Orquestador de provisión dinámica de volumenes sobre glusterfs 

apimanager
  26 - WSO2. Api Manager. → usa java → 4 maquinas

runtime
  17 - Lenguaje de progamación. → java + nodejs [microservicios]
```

install
  5 rhel
		apache - proxy inverso + monitorizado → 2500€
    
integracion F5 + OCP


