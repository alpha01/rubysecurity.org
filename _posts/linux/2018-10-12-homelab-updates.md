---
categories:
  - linux
  - homelab
tags:
  - ubuntu
  - kvm
layout: post
title: Homelab Updates!
created: 1539379113
---

It's been well over a month since I finally decided to retire both of my <a href="https://www.alpha01.org/wp-content/uploads/gallery/main/IMG_20180826_221457.jpg" target="_blank">Apple Mac Minis</a> in favor for a single (for the time being),  quieter, and more powerful <a href="https://www.rubyninja.org/2018/08/29/new-home-lab-hardware-refresh/" target="_blank">Intel NUC</a>.

Migrating over my existing KVM and VirtualBox VMs to my new KVM server was a really easy process. If doing the import manually, then it's just a matter of selecting the existing vdi and qcow2 images as the source disks when creating the guests VMs on the server. In my cause, however I also had to update the new MAC address given that all of my VMs are configured to get their respective fixed IP addresses via my isc-dhcpd server.

This this was somewhat of a fresh start, so I nuked a bunch of unused VMs that I had lingering around for testing purposes, and only kept what I really need for now. Which at the time of this writing these are my current active VMs that I use on my homelab:

* `proxy` - Reverse proxy Varnish and Nginx (SSL termination)
* `dhcp` - ISC-dhcpd and PXE server
* `database` - MySQL and PostgreSQL server
* `monitor` -  Nagios, Graphite/Grafana
* `web` - Apache
* `ns1` - Master BIND server
* `ns2` - Slave BIND server
* `git` -  GitLab and Subversion
* `ansible`  - Ansible and Puppet Configuration Management
* `build` - Jenkins
* `logs` - ELK stack

## Future Plans

I have lots of future plans for my homelab. Like upgrading my BIND DNS servers to a new version and rollout out DNSSec on my local network, upgrading dhcp server (running a really old version of Debian), rollout 389 Directory Server (I have a love/hate relationship with openldap). These are just to name a few!
