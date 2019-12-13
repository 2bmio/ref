---
title: services
description: 
published: true
date: 2019-12-13T23:42:49.105Z
tags: sys
---

# Daemons
also known as background processes

## systemd

> path of the journald configuration file `/etc/systemd/journald.conf`

> e.g.: to remove archived journal files until the disk space they use falls below 100M `SystemMaxUse=100M`

> e.g.: to make all journal files contain no data older than 5 days `MaxFileSec=5day`


### references
```
# journalctl --disk-usage
# journalctl --rotate
# journalctl --vacuum-time=1s
# journalctl --rotate --vacuum-size=500M
```
