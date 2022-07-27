---
categories:
  - awesome-applications
  - apache
tags:
  - apache
  - centos
layout: post
title: 'Apache: Installing mod_pagespeed on CentOS 6'
created: 1363661287
---

### Error:

```bash
rpm -ivh mod-pagespeed-stable_current_x86_64.rpm
warning: mod-pagespeed-stable_current_x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 7fac5991: NOKEY
error: Failed dependencies:
	at is needed by mod-pagespeed-stable-1.2.24.1-2581.x86_64
```

### Fix:

```bash
yum localinstall mod-pagespeed-stable_current_x86_64.rpm
```
