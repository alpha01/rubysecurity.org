---
categories:
  - awesome-applications
  - kerberos
tags:
  - kerberos
  - iptables
layout: post
title: Kerberos - kadmin connection issue
created: 1346474889
---

I was getting a communication error when trying to connect from a Kerberos client to the KDC, while I was still able to successfully be granted a ticket using `kinit`.

### Error

```bash
[root@rubyninja etc]# kadmin -p kerberosadmin/admin@RUBYNINJA.ORG
Authenticating as principal kerberosadmin/admin@RUBYNINJA.ORG with password.
Password for kerberosadmin/admin@RUBYNINJA.ORG: 
kadmin: Communication failure with server while initializing kadmin interface
```

### Fix

It turns out that iptables was blocking access to `kadmind` on the Master KDC, of which I simply had to allow the `TCP Port 749` to fix the issue.

```bash
iptables -A INPUT -p tcp -m tcp --dport 749 -j ACCEPT
```
