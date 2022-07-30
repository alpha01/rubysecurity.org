---
categories:
  - awesome-applications
  - ansible
tags:
  - ansible
  - centos
  - ubuntu
layout: post
title: System Update using Ansible
created: 1412467738
---

### CentOS

```bash
ansible centosbox -m yum -a 'name=* state=latest'
```

### Ubuntu

```bash
ansible debianbox -m apt -a 'update_cache=yes name=* state=latest'
```
