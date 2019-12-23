---
title: System → space
description: linux ephemeral place
published: true
date: 2019-12-23T11:46:50.788Z
tags: sys, space
---

# Header
Your content here


## SWAP

```
dd if=/dev/zero of=/home/swap bs=1024 count=4096k
mkswap /home/swap
    Setting up swapspace version 1, size = 4194300 KiB
    no label, UUID=a19097c6-b413-419f-b8c8-9849e42fa8dc

swapon /home/swap


swapoff /dev/dm-1
swapon /dev/dm-1

swapoff /home/swap
rm -f /home/swap



dd if=/dev/zero of=/swapfile bs=1k count=4096k


top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print 100 - $1"%"}'
ps -e -orss=,args= | sort -b -k1,1n | pr -TW$COLUMNS
ps -eo user,pid,ppid,cmd,%mem,%cpu,stat,start --sort=-%mem | head
```