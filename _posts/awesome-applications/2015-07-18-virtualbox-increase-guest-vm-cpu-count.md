---
categories:
  - awesome-applications
  - virtualbox
tags: virtualbox
layout: post
title: 'VirtualBox: Increase guest VM CPU count'
created: 1437199912
---

### Syntax

VBoxManage modifyvm \<VMNAME\> --cpus \<CPUcount\>

```bash
tony@mini02:~$ VBoxManage showvminfo monitor | grep "Number of CPUs"
Number of CPUs:  1
tony@mini02:~$ VBoxManage modifyvm monitor --cpus 3
tony@mini02:~$ VBoxManage showvminfo monitor | grep "Number of CPUs"
Number of CPUs:  3
```
