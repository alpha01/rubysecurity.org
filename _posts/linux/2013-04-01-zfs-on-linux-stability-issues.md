---
categories:
  - linux
  - zfs
tags:
  - zfs
layout: post
title: 'ZFS on Linux: Stability Issues'
created: 1364783809
---

So far I've had one stability issue on my backup virtual machine. Though, I can't really blame ZFS for crashing my VM, instead I believe this was a consequence the VM running out of memory due to large amount of rsync, and the heavy I/O caused on the ZFS drive.

After updating the dedicated memory on my backup VM from 512 MB to 3.5 GB and updating my rsync's to run with low process and I/O priority, I have yet experience any more problems.

```bash
nice -n 19 ionice -c 3
```
