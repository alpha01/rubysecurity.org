---
categories:
  - linux
  - ubuntu
tags:
  - ubuntu
  - fedora
  - lvm
layout: post
title: Restoring access to Fedora after Ubuntu upgrade
created: 1430639552
---

I have a quadroboot OS installation environment on my Dell XPS laptop.

* Ubuntu (primary OS)
* Kali
* Fedora
* Windows 7

I decided to upgrade my Ubuntu installing to the latest 15.04. As soon the upgrade completed and rebooted, I noticed the GRUB menu was no longer displaying my Fedora 21 environment. The problem was because I had installed Fedora under an LVM partition, while the others weren't.

Restoring boot access to Fedora was fairly simple.

First, I had install `lvm2` package in Ubuntu so it's able to view and configure the LVM

```bash
tony@alpha05:~$ sudo apt-get install lvm2
```

Then I had to activate the Volume Group.

```bash
tony@alpha05:~$ sudo vgchange -a y
```

After updating the Volume Group, using the `os-prober` tool, I was able to verify that Ubuntu was able to see my Fedora 21 install.

```bash
tony@alpha05:~$ sudo os-prober
/dev/sda1:Windows 7 (loader):Windows:chain
/dev/sda6:Debian GNU/Linux (Kali Linux 1.0):Debian:linux
/dev/mapper/fedora-root:Fedora release 21 (Twenty One):Fedora:linux
```

So the last step was to generate a new grub config.

```bash
tony@alpha05:~$ sudo grub-mkconfig > /boot/grub/grub.cfg 
```
