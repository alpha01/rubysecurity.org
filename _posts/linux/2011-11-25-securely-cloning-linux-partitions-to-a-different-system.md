---
categories:
  - linux
  - dd
tags: security
layout: post
title: Securely cloning Linux partitions to a different system
created: 1322189831
---

Getting a bit-by-bit copy of a partition or an entire hard drive is quite simple using the tool `dd`. Depending on what you're trying to accomplish, there are two ways to get a copy of your partitions/drives.

One option is to clone the partition/drive directly to an actual partition or drive of your different system.

```bash
dd if=/dev/sdb1 | ssh root@rubyninja.org of=/dev/sdb1
```

Another option is to clone the partition or drive to a raw file on your second system, and associate the raw file to a loopback on your secondary host. This will give you the ability to easily mount the file as it were an actual partition or drive stored on your drive.

```bash
dd if=/dev/sdb1 | ssh root@rubyninja.org of=/root/rubysecurity_sdb1
```

```bash
root@rubyninja.org # losetup /dev/loop2 /root/rubysecurity_sdb1
root@rubyninja.org # mount /root/rubysecurity_sdb1 /mnt
```
