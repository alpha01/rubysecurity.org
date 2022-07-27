---
categories:
  - linux
  - zfs
tags:
  - ubuntu
  - virtualbox
  - zfs
layout: post
title: 'ZFS on Linux: Kernel updates'
created: 1374984013
---

Just as I would expect, updating both the kernel's of the machine that is running VirtualBox and its virtual machines and the ZFS enabled Linux virtual machine has completed with absolutely no issues.  Originally, I was more concern on updating the host VirtualBox machine's kernel given that I've never really done this in the past using the additional VirtualBox Extension Pack add-on before, while on the other hand I wasn't to concern regarding the ZFS kernel module, given that it was installed as part of a `dkms` kernel module rpm. Which regardless of what people think about `dkms` modules, as a sysadmin that have worked with Linux systems with them (proprietary), it's certainly a relief knowing that little or no additional work is needed to rebuild the respective module after updating to a newer kernel.
