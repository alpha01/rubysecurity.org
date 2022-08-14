---
categories:
  - solaris
  - zfs
tags:
  - solaris
  - zfs
layout: post
title: Creating large files in Solaris for testing purposes
created: 1356392845
---

In the Linux world, I use the `dd` utility to create files that need to be a certain size. Even though it works perfectly fine, its kind of annoying figuring out the output file's size of the file. This is because the size is based on the `bs` (block size) value and the total number of block size `count` together.

For example, the following `dd` command creates a 300 mb file called 300mb-test-file. Each block size will be 1000 bytes, and I want of a total of 300,000 blocks.
Formula: ( (1000 x 300000) / 1000000 )

```bash
[tony@bashninja ~]$ dd if=/dev/zero of=300mb-test-file bs=1000 count=300000
300000+0 records in
300000+0 records out
300000000 bytes (300 MB) copied, 2.0363 s, 147 MB/s
```

Luckily in the Solaris world this can be easily accomplished using the `mkfile` tool, without doing any conversion. We can even use this `mkfile` tool to easily create test disk files to experiment with ZFS!

```bash
root@solaris:~# mkfile 300m testdisk1
root@solaris:~# mkfile 300m testdisk2
root@solaris:~# ln -s /root/testdisk1 /dev/dsk/testdisk1
root@solaris:~# ln -s /root/testdisk2 /dev/dsk/testdisk2
root@solaris:~# zpool create tonytestpool mirror testdisk1 testdisk2
root@solaris:~# zpool status tonytestpool
  pool: tonytestpool
 state: ONLINE
  scan: none requested
config:

        NAME           STATE     READ WRITE CKSUM
        tonytestpool   ONLINE       0     0     0
          mirror-0     ONLINE       0     0     0
            testdisk1  ONLINE       0     0     0
            testdisk2  ONLINE       0     0     0

errors: No known data errors
```
