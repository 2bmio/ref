---
title: System → storage
description: linux ephemeral place and storage
published: true
date: 2020-01-09T13:58:53.051Z
tags: sys, storage
---

# Header
Your content here

# memory

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
