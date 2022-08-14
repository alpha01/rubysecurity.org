---
categories:
  - linux
  - centos
tags:
  - centos
  - ubuntu
  - networking
layout: post
title: DHCP configurations in Red Hat/CentOS and Debian/Ubuntu
created: 1329688354
---

### Example for eth0

Red Hat/CentOS: `/etc/sysconfig/network-scripts/ifcfg-eth0`

```bash
BOOTPROTO=dchp
```

Ubuntu/Debian: `/etc/networks/interfaces`

```bash
iface eth0 inet dhcp
```
