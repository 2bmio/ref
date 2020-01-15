---
title: Rundeck→ home
description: rundeck
published: true
date: 2020-01-15T12:36:53.403Z
tags: iac, rundeck
---



```
https://geekdudes.wordpress.com/2018/02/09/creating-rundec-acl-policies/

ejemplo crear usuario salesforce:

/etc/rundeck/salesforce.aclpolicy
/etc/rundeck/realm.properties


[root@libs01 rundeck]# cat /etc/rundeck/salesforce.aclpolicy
description: salesforce, all access.
context:
  project: 'PibankTest' # all projects
for:
  resource:
    - equals:
        kind: event
      allow: [read]
  adhoc:
    - deny: [read,running,killing] # allow read/running/killing adhoc jobs
  job:
    - equals:
        name: 'chmod_tmp_sdl.log'
      allow: [read,run,kill] # allow read/write/delete/run/kill of all jobs
  node:
    - allow: [read,run]
by:
  group: salesforce

---

description: salesforce, all access.
context:
  application: 'rundeck'
for:
  resource:
    - equals:
        kind: project
      deny: [create] # deny create of projects
    - equals:
        kind: system
      deny: [read] # allow read of system info
    - equals:
        kind: user
      deny: [admin] # allow modify user profiles
  project:
    - equals:
        name: 'PibankTest'
      allow: [read] # allow access
      deny: [import,export,configure,delete] # deny admin actions
  storage:
    - allow: [read]
    - deny: [admin,create] # allow access for /keys/* storage content
by:
  group: salesforce

```