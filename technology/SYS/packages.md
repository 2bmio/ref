---
title: System → packages
description: 
published: true
date: 2020-01-24T08:00:46.237Z
tags: sys, packages
---

# commons packages configs


## get a rpm list

```
mkdir -p /root/explotacion/anibal/installed-rpm
cd /root/explotacion/anibal/installed-rpm
rpm -qa > installed-rpm-[node-name].txt
scp root@[node-name]:/root/explotacion/anibal/installed-rpm/installed-rpm-[node-name].txt .



```


## ICINGA2

disable/enable monitor

```
# login to icinga server
root@icinga-server

# get defaults values
icinga2 --version

# locate target
grep -r "nrpe-vault-pro" /etc/icinga2
    /etc/icinga2/conf.d/hosts-pichincha/libs01-external-services.conf:object Service "nrpe-vault-pro/" {

# edite icinga congif
vi /etc/icinga2/conf.d/hosts-pichincha/libs01-external-services.conf
    # object Service "nrpe-vault-pro/" {
    #  import "generic-service"
    #  host_name = "libs01.ocs.inet"
    #  check_command = "nrpe"
    #  vars.nrpe_command = "check-vault-pro"
    # }

# restart service
systemctl restart icinga2
```