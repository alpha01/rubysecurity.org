---
categories:
  - linux
  - kvm
tags:
  - kvm
  - php
  - centos
  - iptables
  - apache
layout: post
title: Server Move and Upgrades!
created: 1434951190
---

My little corner of the internet has a new home. My old $29.99 8GB RAM, 3.40GHz Intel Core i3 dedicated server was simply not enough to handle my server needs. Which apparently OVH doesn't even provide that service anymore. So instead I hoped to their mid-tear dedicated service service branch they call <a href="https://www.soyoustart.com/us/" target="_blank">So you Start</a>. I opted with their $49.00 <a href="https://www.soyoustart.com/us/offers/sys-ip-2.xml" target="_blank">SYS-IP-2</a> service. Now my server's specs is a follows:

* 2.66 GHz+ Intel Xeon W3520 (4 cores/ 8 threads)
* 32 GB ECC
* 2 x 2 TB SATA drives (Software RAID)

I would've love the drives to be SAS and the RAID to be hardware based, but it's definitely not a deal breaker, and just $49.99 a month, it's not much to complain about.

### CentOS 6 to CentOS 7 upgrade

My server migration was fairly straight forward for the most part. I opted to re-create the KVM hypervisor and its guests from scratch. Mainly because I wanted to upgrade all of guests and host from CentOS 6 to CentOS 7.  This is where I encountered my first problem. Since I rely on custom nat `PREROUTING` and `POSTROUTING` iptables firewall rules for my VMs to properly be able to talk to each other and to the internet. I realized CentOS 7 defaults to firewalld, so instead of trying to rewrite my firewall rules to be compatible with firewalld, I decided to continue to use CentOS 6 on my host operating system, and only upgrade my guests VMs to CentOS 7.

On a side note, my previous guest VMs were originally using raw image format (default cache settings) for its storage, and by god what a hell of a difference it makes changing to use native block storage via LVM.  I/O performance on my old server was terrible, the I/O wait percentage was roughly about 6%, now it's less than 1%. Even with the software raid, I/O performance is much better on my new server.

### PHP 5.3 to 5.6 upgrade

Since I don't have anything heavily customized on any of sites, the PHP version upgrade was practically painless.

### Apache 2.2 to 2.4 upgrade

Luckily, upgrading Apache wasn't a big hassle.  Anyone considering upgrading from 2.2 to 2.4,  it's definitely worth checking out the <a href="http://httpd.apache.org/docs/2.4/upgrading.html" target="_blank">official upgrade documentation</a> since dropping the old 2.2 configs in onto a 2.4 environment won't work off the gecko. In my case all of my sites were returning 403 forbidden replies and non of my .htaccess files weren't being read by Apache. The fix was really simple.

```bash
<Directory /www/path-to-webroot>
    AllowOverride All
    Require all granted
</Directory>
```

I must say, I really like Apache 2.4 new authorization syntax. What used to be a three line configuration is now a single line configuration, and much more human readable.

### Future Upgrade Plans

I didn't tackle this during the server migration, but I'll definitely going to be upgrading to Varnish 4 and use PHP FastCGI via `php-fpm` and `mod_proxy_fcgi`.
