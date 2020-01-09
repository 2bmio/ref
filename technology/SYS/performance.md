---
title: System → performance
description: performance and maintenance
published: true
date: 2020-01-09T12:39:29.458Z
tags: sys, performance
---

# Monitoring

## MEMORY usage

```
ps -eo user,pid,ppid,cmd,%mem,%cpu,stat,start --sort=-%mem | head

continuous reporting
watch "ps -eo user,pid,ppid,cmd,%mem,%cpu,stat,start --sort=-%mem | head"


$ find /proc -maxdepth 2 -path "/proc/[0-9]*/status" -readable -exec awk -v FS=":" -v TOTSWP="$(cat /proc/swaps | sed 1d | awk 'BEGIN{sum=0} {sum=sum+$(NF-2)} END{print sum}')" '{process[$1]=$2;sub(/^[ \t]+/,"",process[$1]);} END {if(process["VmSwap"] && process["VmSwap"] != "0 kB") {used_swap=process["VmSwap"];sub(/[ a-zA-Z]+/,"",used_swap);percent=(used_swap/TOTSWP*100); printf "%10s %-30s %20s %6.2f%\n",process["Pid"],process["Name"],process["VmSwap"],percent} }' '{}' \;  | awk '{print $(NF-2),$0}' | sort -hr | head | cut -d " " -f2-

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
df -h  | sort -nr

du -sm * | sort -nr
```