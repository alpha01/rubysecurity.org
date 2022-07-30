---
categories:
  - linux
  - centos
tags: centos
layout: post
title: Enabling NFTS read/write support on CentOS 5 Live CD
created: 1327392271
---

```bash
rpm -Uvh http://download.fedora.redhat.com/pub/epel/5/i386/epel-release-5-4.noarch.rpm
yum install fuse ntfs-3g
```
