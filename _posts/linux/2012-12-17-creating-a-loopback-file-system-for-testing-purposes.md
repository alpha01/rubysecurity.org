---
categories:
  - linux
tags: testing
layout: post
title: Creating a loopback file system for testing purposes
created: 1355732194
---

Using the all mighty `dd` tool, this example the block size is 1024 bytes and a total of 5000 blocks:

```bash
dd if=/dev/zero of=/tmp/temploopbackimage.img bs=1024 count=5000
```

Associate the image file to the loopback device

```bash
losetup /dev/loop3 /tmp/temploopbackimage.img
```

Now you should be able to format `/dev/loop3`
