---
categories:
  - cloud
  - google
tags: google
layout: post
title: Leaving Gmail and Google Apps
created: 1384683583
---

Ever since finding out about Google's involvement in <a href="http://en.wikipedia.org/wiki/PRISM_(surveillance_program)" target="_blank">PRISM</a> a few months back, I've been wanting to completely ditch their Gmail and Google Docs services for good. Having used those services for such a long time, and more importantly them being free (as in beer), deciding which new platform I would use as the replacement was my first challenge. Firstly, all email services will be managed by me solely.  So my first task was to find a reliable, and perhaps more importantly, a really cheap unmanaged dedicated hosting provider. I opted to go with <a href="http://www.ovh.com/us/dedicated-servers/pe1.xml" target="_blank">OVH</a>. Having first heard of OVH on a Linux Journal advertisement, I became completely sold on their $29.99 a month dedicated hosting offering. Even better, OVH is not an American company and they themselves having to deal with lots of scrutiny for hosting WikiLeaks. The really cheap $29.99 dedicated hosting plan does have a catch, the hardware is not enterprise quality server hardware, but rather desktop hardware. The disk is not RAIDed, and the memory is non ECC. Personally, since I don't believe my server will have much heavy load, I really don't see this as a problem. Additionally, using my home Nagios monitoring server and with the help of NRPE, I'm going to monitoring just about everything on the dedicated machine.


### My current hypothetical architecture

Install configure KVM on the system, and run three virtual machine instances. The host machine will have the following:

* Varnish, to proxy all HTTP traffic
* Nginx, to proxy all HTTPS traffic
* Proxy POP/IMAP mail traffic using iptables (if this does not work as I expect, I might look into using haproxy instead)

1). VM 1: http

* Apache PHP/Ruby, all of my web apps will be using on this VM. (including this site itself)

2). VM 2: database
* MySQL
* PostgreSQL

3). VM 3: email
* Postfix for sending mail
* Dovecot for receiving mail
