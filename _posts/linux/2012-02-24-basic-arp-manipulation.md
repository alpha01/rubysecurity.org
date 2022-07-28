---
categories:
  - linux
  - networking
tags:
  - networking
  - security
layout: post
title: Basic ARP Manipulation
created: 1330063036
---
Add a temporary ARP entry to a hosts ARP cace

```bash
arp -s 192.168.1.145 00:02:03:F6:7C:4B temp
```

Permanent static entry (not temporary: NO “temp”)

```bash
arp -s 192.168.1.145 00:02:03:F6:7C:4B
```
