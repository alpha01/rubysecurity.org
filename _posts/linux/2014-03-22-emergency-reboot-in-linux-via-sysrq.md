---
categories:
  - linux
  - sysrq
tags: monitoring
layout: post
title: Emergency reboot in Linux via SysRq
created: 1395531998
---

When your Linux system has completely shit itself, and an emergency reboot needs to be made. Linux Magic System Request Keys to the rescue.

```bash
[root@server1 ~]# echo "1" > /proc/sys/kernel/sysrq
[root@server1 ~]# echo "b" > /proc/sysrq-trigger
```

Resources:
* <a href="https://www.kernel.org/doc/Documentation/sysrq.txt" target="_blank">https://www.kernel.org/doc/Documentation/sysrq.txt</a>
