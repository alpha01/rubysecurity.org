---
categories:
  - awesome-applications
  - smartmontools
tags:
  - monitoring
layout: post
title: Enabling SMART on a hard drive
created: 1384110520
---

### Error

```bash
[root@backup ~]# smartctl -H /dev/sdb
smartctl 5.43 2012-06-30 r3573 [x86_64-linux-2.6.32-358.23.2.el6.x86_64] (local build)
Copyright (C) 2002-12 by Bruce Allen, http://smartmontools.sourceforge.net

SMART Disabled. Use option -s with argument 'on' to enable it.
```

### Fix

```bash
[root@backup ~]# smartctl -s on /dev/sdb
smartctl 5.43 2012-06-30 r3573 [x86_64-linux-2.6.32-358.23.2.el6.x86_64] (local build)
Copyright (C) 2002-12 by Bruce Allen, http://smartmontools.sourceforge.net

=== START OF ENABLE/DISABLE COMMANDS SECTION ===
SMART Enabled.
```
