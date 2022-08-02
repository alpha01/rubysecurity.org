---
categories:
  - linux
  - networking
tags:
  - ubuntu
  - centos
layout: post
title: Static IP configurations in Red Hat/CentOS and Debian/Ubuntu
created: 1328087234
---

### Static IP configurations

Ubuntu/Debian: `/etc/networks/interfaces`

```bash
iface eth0 inet static
address 192.168.1.100
netmask 255.255.255.0
network 192.168.1.0
broadcast 192.168.1.255
gateway 192.168.1.254
```

RedHat/Centos: `/etc/sysconfig/network-scripts/ifcfg-eth0`

```bash
DEVICE=eth0
BOOTPROTO=static
IPADDR=192.168.1.140
BROADCAST=192.168.1.255
NETMASK=255.255.255.0
NETWORK=192.168.1.0
ONBOOT=yes
```
