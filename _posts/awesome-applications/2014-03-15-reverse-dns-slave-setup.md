---
categories:
  - awesome-applications
  - bind
tags: bind
layout: post
title: Reverse DNS Slave Setup
created: 1394921164
---

So a few months back, <a href="/awesome-applications/bind/reverse-dns-in-bind-9-8" target="_blank">I enabled reverse DNS on my home BIND server.</a> One thing that I forgot to implement was the additional slave DNS reverse setup. Like many things in BIND, the slave reverse setup was a dead simple process.

It's simply just a matter of adding the following entry to the slave's `named.conf` with the updated master's DNS IP specified in the masters directive and reload BIND.

```bash
zone "1.168.192.in-addr.arpa" IN {
        type slave;
        file "etc/zones/db.192.168.1.255.bak";
        allow-query { any; };
        masters { MasterDNSIP; };
};
```
