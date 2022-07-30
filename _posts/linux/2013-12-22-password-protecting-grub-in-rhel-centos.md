---
categories:
  - linux
  - centos
tags:
  - centos
  - rhel
  - security
layout: post
title: Password protecting GRUB in RHEL/CentOS
created: 1387753085
---

Specifying a password to modify GRUB during the boot start-up phase can be initially set during the install, but it can also be manually added and or modified after the installation.

Using the `grub-md5-crypt` utility, you can generate an md5 hashed password (some security better than no security). 

```bash
[root@centos6 ~]# grub-md5-crypt 
Password: 
Retype password: 
$1$/dvPV1$ngGsOO21eHj2lzEk7wg9d0
```

Now, is just a matter of adding the following entry in `/boot/grub/grub.conf`.

```bash
password --md5 $1$/dvPV1$ngGsOO21eHj2lzEk7wg9d0
```

Restart, and voala.

<img src="/assets/linux/grub.png" alt="GRUB image" title="GRUB image"> 
