---
categories:
  - linux
  - grub
tags:
  - centos
  - security
layout: post
title: Password protecting single user mode
created: 1388206964
---

I was surprise to find out how easy it was to password protect runlevel 1 aka single user mode in RHEL/CentOS.

Simply update the SINGLE variable in the file `/etc/sysconfig/init`

```bash
SINGLE=/sbin/sulogin
```

<img src="/assets/linux/runlevel.png" alt="Single User mode password protected" title="runlevel-1">

If the root password cannot be retrieved/reset, then at this point the only option will be to boot into a rescue environment, assuming encryption hasn't been enabled.
