---
title: performance
description: performance and maintenance
published: true
date: 2019-12-18T21:38:35.705Z
tags: sys, performance
---

# Monitoring

## MEMORY usage

```
ps -eo user,pid,ppid,cmd,%mem,%cpu,stat,start --sort=-%mem | head

continuous reporting
watch "ps -eo user,pid,ppid,cmd,%mem,%cpu,stat,start --sort=-%mem | head"
```

## CPU usage

```
ps -eo user,pid,ppid,cmd,%mem,%cpu,stat,start --sort=-%cpu | head

#continuous reporting
watch "ps -eo user,pid,ppid,cmd,%mem,%cpu,stat,start --sort=-%cpu | head"
```

## SPACE usage

```
du -ksh * | sort -nr
du -sm * | sort -nr
```