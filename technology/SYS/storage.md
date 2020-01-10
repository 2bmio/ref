---
title: System → storage
description: linux ephemeral place and storage
published: true
date: 2020-01-10T10:01:51.535Z
tags: sys, storage
---

# Header
Your content here

# memory

## SWAP

```
#####################################################################################################
MULTIPLE SWAP
#############

# show initial
swapon --show
NAME      TYPE      SIZE USED PRIO
/dev/dm-1 partition   2G   0B   -2

# create swap file
dd if=/dev/zero of=/extra-swap bs=2G count=1

# check it
ls -lh /extra-swap

# change permissions
chmod 600 /extra-swap

# make the swap
mkswap /extra-swap

# make available it → as you can see the priority is 100 it's mean higter numbers is maximun priority, in this case the swap file is used before the swap partition.
swapon --priority 100 /extra-swap

# check if it's up and running
[root@centos7 /]# free -m
              total        used        free      shared  buff/cache   available
Mem:           3.6G        251M        2.2G        8.5M        1.2G        3.1G
Swap:          4.0G        520K        4.0G

# do the changes permanent → it's mean if the system goes rebooted the extra swap go up!
vi /etc/fstab
/extra-swap swap swap defaults,pri=100 0 0

# if you what check it, reboot the system and look arround..
sync
init 6

# login again and check, cheers!
swapon --show
[root@centos7 ~]# swapon --show
NAME        TYPE      SIZE USED PRIO
/extra-swap file        2G   0B  100
/dev/dm-1   partition   2G   0B   -2

#####################################################################################################


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

### 



# LVM

Configure volume | it will be accessible at `/mnt/volume-nbg1-1`.

```
# Format volume
sudo mkfs.xfs -F /dev/disk/by-id/scsi-0HC_Volume_3858306

# Create directory
mkdir /mnt/volume-nbg1-1

# Mount volume
mount -o discard,defaults /dev/disk/by-id/scsi-0HC_Volume_3858306 /mnt/volume-nbg1-1

#Optional: Add volume to fstab
echo "/dev/disk/by-id/scsi-0HC_Volume_3858306 /mnt/volume-nbg1-1 xfs discard,nofail,defaults 0 0" >> /etc/fstab
```
