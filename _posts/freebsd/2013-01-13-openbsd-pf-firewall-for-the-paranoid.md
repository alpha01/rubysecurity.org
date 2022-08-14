---
categories:
  - openbsd
  - pf
tags: pf
layout: post
title: 'OpenBSD: PF firewall for the paranoid'
created: 1358067746
---

Block all traffic except for ssh.

`/etc/pf.conf`

```bash
tcp_services = "{ 22 }"
block all
pass out on em0 proto tcp to any port $tcp_services keep state
pass in on em0 proto tcp to any port $tcp_services keep state
```

Enabling rules:

```bash
pfctl -e ; pfctl -f /etc/pf.conf 
pfctl: pf already enabled
```
