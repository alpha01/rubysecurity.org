---
categories:
  - linux
  - iptables
tags:
  - iptables
  - networking
layout: post
title: Logging iptables rules
created: 1360725321
---

When debugging certain custom firewall rules, it can sometimes be extremely useful log the rule's activity.
For example, the following rule logs all input firewall activity. The logs will be available via dmesg or syslog.

```bash
iptables -A INPUT -j LOG --log-prefix " iptables INPUT "
```
