---
categories:
  - linux
  - centos
tags:
  - centos
  - monitoring
layout: post
title: Setting up Graphite on CentOS 6.x gotcha
created: 1442038975
---

I installed graphite-web via the EPEL repo, and I was getting an 500 error when accessing the Graphite web interface.

### Error

```bash
[Sat Sep 12 00:56:27 2015] [error] [client 192.168.1.21] mod_wsgi (pid=17318): Exception occurred processing WSGI script '/usr/share/graphite/graphite-web.wsgi'.
[...]
[Sat Sep 12 00:56:27 2015] [error] [client 192.168.1.21]   File "/usr/lib/python2.6/site-packages/django/db/backends/sqlite3/base.py", line 344, in execute
[Sat Sep 12 00:56:27 2015] [error] [client 192.168.1.21]     return Database.Cursor.execute(self, query, params)
[Sat Sep 12 00:56:27 2015] [error] [client 192.168.1.21] DatabaseError: attempt to write a readonly database
```

### Fix

It turns out the `sqlite3` database file Graphite write's too, was owned by root. So it was simply a matter of updating the ownership to what ever user Apache is running under, in my case it's apache.

```bash
chown -R apache.apache /var/lib/graphite-web/
```
