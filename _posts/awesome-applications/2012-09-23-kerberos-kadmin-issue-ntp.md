---
categories:
  - awesome-applications
  - kerberos
tags:
  - kerberos
  - ntp
layout: post
title: Kerberos - Kadmin issue NTP
created: 1348373390
---

I stumbled onto yet another Kerberos problem.

### Error

```bash
[root@afs2 log]# kadmin -p kerberosadmin/admin@RUBYNINJA.ORG
Authenticating as principal kerberosadmin/admin@RUBYNINJA.ORG with password.
Password for kerberosadmin/admin@RUBYNINJA.ORG: 
kadmin: GSS-API (or Kerberos) error while initializing kadmin interface
```

### Fix

Make sure the time is correct on your Kerberos client/server, ideally NTP should be enabled on the hosts to avoid things like these from happening. Until I rollout ntp on my local LAN, I just manually ran `ntpdate`

```bash
ntpdate us.pool.ntp.org
```
