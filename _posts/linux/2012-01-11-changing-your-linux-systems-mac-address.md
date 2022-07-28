---
categories:
  - linux
  - networking
tags:
  - networking
  - security
layout: post
title: Changing your Linux system's mac address
created: 1326260875
---

```bash
ifconfig eth0 down hw ether 00:00:00:00:00:01
ifconfig eth0 up
```
