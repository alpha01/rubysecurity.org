---
categories:
  - linux
  - ubuntu
tags: ubuntu
layout: post
title: Installing system-config-kickstart on Ubuntu
created: 1429995367
---

`system-config-kickstart` fails to start after the initial install.

### Error

```bash
tony@alpha05:~$ system-config-kickstart 
Traceback (most recent call last):
  File "/usr/share/system-config-kickstart/system-config-kickstart.py", line 92, in <module>
    kickstartGui.kickstartGui(file)
  File "/usr/share/system-config-kickstart/kickstartGui.py", line 131, in __init__
    self.X_class = xconfig.xconfig(xml, self.kickstartData)
  File "/usr/share/system-config-kickstart/xconfig.py", line 80, in __init__
    self.fill_driver_list()
  File "/usr/share/system-config-kickstart/xconfig.py", line 115, in fill_driver_list
    raise RuntimeError, (_("Could not read video driver database"))
RuntimeError: Could not read video driver database
```

### Fix

Downgrade the hwdata package.

```bash
apt-get remove hwdata
wget ftp://mirror.ovh.net/mirrors/ftp.debian.org/debian/pool/main/h/hwdata/hwdata_0.234-1_all.deb
dpkg -i hwdata_0.234-1_all.deb
apt-mark hold hwdata
apt-get install system-config-kickstart
```

This is a known bug in Ubuntu that is yet to be fixed...

### Resources

* <a href="https://bugs.launchpad.net/ubuntu/+source/system-config-kickstart/+bug/1260107" target="_blank">https://bugs.launchpad.net/ubuntu/+source/system-config-kickstart/+bug/1260107</a>
* <a href="https://bugs.launchpad.net/ubuntu/+source/system-config-kickstart/+bug/1236315" target="_blank">https://bugs.launchpad.net/ubuntu/+source/system-config-kickstart/+bug/1236315</a>
