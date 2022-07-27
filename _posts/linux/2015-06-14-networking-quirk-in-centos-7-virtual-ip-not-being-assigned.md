---
categories:
  - linux
  - centos
tags:
  - centos
  - networking
layout: post
title: Networking Quirk in CentOS 7 - Virtual IP not being assigned
created: 1434259549
---

I just realized the order of which the IP configurations are set in the `/etc/sysconfig/networking-scripts/ifcfg-*` file does matter. For example the following config was failing to assign the virtual IP 192.168.100.218 on one of my systems:

```bash
TYPE="Ethernet"
BOOTPROTO="static"
IPADDR=192.168.100.218
NETMASK=255.255.255.0
DEVICE="ens3:1"
NAME="ens3:1"
ONBOOT="yes
```

Systemd was spitting out the following errors:

```bash
Jun 14 01:04:19 webapps network: RTNETLINK answers: File exists
Jun 14 01:04:19 webapps network: RTNETLINK answers: File exists
Jun 14 01:04:19 webapps network: RTNETLINK answers: File exists
Jun 14 01:04:19 webapps network: RTNETLINK answers: File exists
```

### Fix

It turns out that the `DEVICE` and `NAME` declaration needs to be assigned and specified before the networking information.

```bash
DEVICE="ens3:1"
TYPE="Ethernet"
NAME="ens3:1"
BOOTPROTO="static"
IPADDR=192.168.100.218
NETMASK=255.255.255.0
ONBOOT="yes"
</blockquote>
```