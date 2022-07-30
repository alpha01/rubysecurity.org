---
categories:
  - linux
  - opensuse
tags:
  - opensuse
layout: post
title: Problems installing Chrome on OpenSuSE 13.1
created: 1388983566
---

### Error

```bash
linux-5n99:/home/tony/Downloads # rpm -ivh google-chrome-stable_current_x86_64.rpm
warning: google-chrome-stable_current_x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 7fac5991: NOKEY
error: Failed dependencies:
	lsb >= 4.0 is needed by google-chrome-stable-31.0.1650.63-1.x86_64
```

### Fix

```bash
linux-5n99:/home/tony/Downloads # yast --install lsb
```
