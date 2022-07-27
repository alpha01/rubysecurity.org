---
categories:
  - awesome-applications
  - nagios
tags:
  - networking
  - nagios
  - iptables
layout: post
title: Monitoring DHCP server with check_dhcp
created: 1360041744
---

Setting Nagios to monitor my DHCP server using the plugin check_dhcp was a little tricky to setup.

First, the check_dhcp documentation indicates setting setuid on the `check_dhcp` binary in order to successfully query the dhcp server and receive a valid dhcp offer.

```bash
root@monitor libexec]# su - nagios -c '/usr/local/nagios/libexec/check_dhcp -s 192.168.1.2'
Warning: This plugin must be either run as root or setuid root.
To run as root, you can use a tool like sudo.
To set the setuid permissions, use the command:
	chmod u+s yourpluginfile
Error: Could not bind socket to interface eth0.  Check your privileges...
```

## Fix:

```bash
chown root.root check_dhcp 
chmod u+s check_dhcp 
```

Secondly, since I always have all of my machines block all incoming traffic, I had to open up the `UDP Port 68` in order for the Nagios machine to accept the dhcp offer.

```bash
iptables -A INPUT -p udp --dport 68 -j ACCEPT
```
