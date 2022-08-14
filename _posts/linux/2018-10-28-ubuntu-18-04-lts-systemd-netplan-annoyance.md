---
categories:
  - linux
  - ubuntu
tags:
  - ubuntu
  - systemd
  - networking
layout: post
title: Ubuntu 18.04 LTS + Systemd + Netplan = Annoyance
created: 1540697817
---

Unless it's something that is suppose to help improve workflow, I really hate change; especially if the change involves changing something that worked perfectly fine.

I upgraded (fresh install) from Ubuntu Server LTS 12.04 to 18.04, among the addition of systemd, which I don't mind to be honest, as I see it as necessary evil. I was shocked to see the old traditional Debian networking configuration does not work anymore. Instead, networking is handled by a new utility called <a href="https://netplan.io/" target="_blank">Netplan</a>. Using Netplan for normal static networking configurations is not terrible, however in my use-case, I needed to able to create a new virtual interface for the shared KVM bridge networking config needed for my guest VMs.

After about 30 minutes of trail and error (and wasn't able to find any useful documentation), I opted to configure the networking config to continue using the old legacy networking config. The only problem is that reverting to my old 12.04 networking config was not quite as easy as simply copying over the old `interfaces` file. So I had to do the following:

1). Remove all of the configs on `/etc/netplan/`

```bash
rm  /etc/netplan/*.yml
```

2). Install ifupdown utility

```bash
sudo apt install ifupdown
```

Now, populate your `/etc/network/interfaces` config. This is how mine looks (where eno1 is my physical interface):

```bash
# ifupdown has been replaced by netplan(5) on this system.  See
# /etc/netplan for current configuration.
# To re-enable ifupdown on this system, you can run:
#    sudo apt install ifupdown
#
auto lo
iface lo inet loopback

 auto br0
 iface br0 inet static
         address 192.168.1.25
         netmask 255.255.255.0
         dns-nameservers 8.8.8.8 192.168.1.10 192.168.1.11
         gateway 192.168.1.1
         # set static route for LAN
         post-up route add -net 192.168.0.0 netmask 255.255.255.0 gw 192.168.1.1
         bridge_ports eno1
         bridge_stp off
         bridge_fd 0
         bridge_maxwait 0
```

After restarting the network service, my new shared interface was successfully created with the proper IP Address and routing, however DNS was not configured. This is because now DNS configurations seem to have their own dedicated tool called `systemd-resolved`. So to get my static DNS configured and working on the half-ass networking legacy configuration. Using `systemd-resolved` is a two step process:

1). Update the file `/etc/systemd/resolved.conf` with the corresponding DNS configuration, in my case it looks like this:

```bash
[Resolve]
DNS=192.168.1.10
DNS=192.168.1.11
DNS=8.8.8.8
Domains=rubyninja.org
```

2). Then finally restart the systemd-resolved service.

```bash
systemctl restart systemd-resolved
```

You can verify the DNS config using

```bash
systemd-resolve --status
```

It wasn't easy as I first imagined, but thus said, this was the only inconvenience during my entire 12.04 to 18.04 upgrade.
