---
categories:
  - awesome-applications
  - bind
tags: bind
layout: post
title: Reverse DNS in BIND 9.8
created: 1378335079
---

I use BIND on my home network, and giving the bast amount of virtual machines I have online, I've always find myself wanting to easily look up which machine is using which IP address without having to ssh into the actual vm or check the zone file. Configuring reverse DNS in BIND 9.8 is actually a dead simple process.

First, a separate zone file for PTR records needs to be created, I named mine `db.192.168.1.255`.
Note: since my network address space is 192.168.1, the actual PTR record will be the network address backgrounds followed by `in-addr.arpa.`.

```bash
$TTL 3h
@       IN SOA  ns1.rubyninja.org. dnsadmin.rubysecurity.org. (
                                        2013090701      ; serial
                                        3h              ; refresh after 3 hours
                                        1h              ; retry after 1 hour
                                        1w              ; expire after 1 week
                                        1H )            ; negative caching TTL of 1 hour

              IN      NS      ns1.rubyninja.org.
              IN      NS      ns2.rubyninja.org.

14.1.168.192.in-addr.arpa.      IN      PTR     email.rubyninja.org.
```

Lastly, the zone entry needs to be added to the master named.conf file. Mine looks like this

```bash
zone "1.168.192.in-addr.arpa" IN {
        type master;
        file "etc/zones/db.192.168.1.255";
        allow-query { any; };
};
```

After reloading Bind, you verify reverse DNS works by using the utility of your choice; ie dig, host, nslookup, etc..

```bash
alpha03:~ tony$ nslookup 192.168.1.14
Server:		192.168.1.10
Address:	192.168.1.10#53

14.1.168.192.in-addr.arpa	name = email.rubyninja.org.
```
