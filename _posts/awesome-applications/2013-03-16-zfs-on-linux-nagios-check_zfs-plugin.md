---
categories:
  - awesome-applications
  - nagios
tags:
  - tags
  - perl
  - nagios
  - zfs
layout: post
title: 'ZFS on Linux: Nagios check_zfs plugin'
created: 1363409947
---

To monitor my ZFS pool, of course I'm using Nagios, duh. Nagios Exchange provide a `check_zfs` plugin written in Perl. http://exchange.nagios.org/directory/Plugins/Operating-Systems/Solaris/check_zfs/details

Although the plugin was originally designed for Solaris and FreeBSD systems, I got it to work under my Linux system with very little modification. The code can be found on my SysAdmin-Scripts git repo on my <a href="https://github.com/alpha01/SysAdmin-Scripts/blob/master/nagios-plugins/check_zfs" target="_blank">GitHub</a> account.

```bash
[root@backup ~]# su - nagios -c "/usr/local/nagios/libexec/check_zfs backups 3"
OK ZPOOL backups : ONLINE {Size:464G Used:11.1G Avail:453G Cap:2%} <sdb:ONLINE>
```
