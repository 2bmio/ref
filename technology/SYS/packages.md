---
title: System → packages
description: 
published: true
date: 2020-01-23T20:55:15.020Z
tags: sys, packages
---

# commons packages configs


## get a rpm list

```
rpm -qa > installed-rpm.txt
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