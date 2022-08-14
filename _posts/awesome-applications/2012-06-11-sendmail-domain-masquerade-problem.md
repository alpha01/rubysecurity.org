---
categories:
  - awesome-applications
  - sendmail
tags:
  - sendmail
layout: post
title: Sendmail domain masquerade problem
created: 1339384501
---

Recently, I wanted to change my host outgoing domain email address. Since I've been getting into the habit of using bare minimal installs, it turns out that sendmail by default does not include `sendmail-cf` package needed by the m4 utility to generate a sendmail `.cf` configuration file.

### Error

```bash
[root@bashninja mail]# m4 < sendmail.mc > sendmail.cf 
m4:stdin:10: cannot open `/usr/share/sendmail-cf/m4/cf.m4': No such file or directory
```

### Fix

```bash
[root@bashninja mail]# yum install sendmail-cf
```
