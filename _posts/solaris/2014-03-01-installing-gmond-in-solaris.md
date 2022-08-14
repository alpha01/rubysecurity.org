---
categories:
  - solaris
  - ganglia
tags:
  - solaris
  - ganglia
layout: post
title: Installing gmond in Solaris 11
created: 1393651095
---
Package is installed using <a href="http://www.opencsw.org/" target="_blank">OpenCSW</a>

Install the installation source

```bash
root@solaris:~# pkgadd -d http://get.opencsw.org/now
```

I updated my PATH via `~/.profile`

```bash
export PATH=/usr/bin:/usr/sbin:/opt/csw/bin
```

Install the `CSWgangliaagent` package

```bash
root@solaris:~# pkgutil --install CSWgangliaagent
```

Enable the service in SMF

```bash
root@solaris:~# svcadm enable svc:/network/cswgmond:default
```
