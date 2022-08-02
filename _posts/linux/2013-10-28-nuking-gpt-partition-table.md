---
categories:
  - linux
  - gparted
tags: gparted
layout: post
title: Nuking GPT partition table
created: 1382942852
---

### Error

```bash
WARNING: GPT (GUID Partition Table) detected on '/dev/sdb'! The util fdisk doesn't support GPT. Use GNU Parted.
```

### Fix

```bash
parted /dev/sdb
mklabel msdos
quit
```
