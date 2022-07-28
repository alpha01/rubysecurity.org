---
categories:
  - awesome-applications
  - apache
tags:
  - apache
  - varnish
layout: post
title: Log Varnish/proxy and Local Access Separately in Apache
created: 1541295759
---

I use Varnish on all of my web sites, with Apache as the backend web server. All Varnish traffic that hits my sites, is traffic that originates from the internet, while all access from my local home network hits Apache directly (Accomplished using local BIND authoritative servers).

For the longest time, I've been logging all direct Apache traffic and traffic originating from Varnish to the same Apache access file. It turns out, segmenting the access logs is a very easy task. This can be accomplish, with the help of environment variables in Apache using **<a href="https://httpd.apache.org/docs/trunk/mod/mod_setenvif.html" target="_blank">SetEnvIf</a>**.

For example, my Varnish server's local IP is 192.168.1.150, and **<a href="https://httpd.apache.org/docs/trunk/mod/mod_setenvif.html" target="_blank">SetEnvIf</a>** can use **_Remote_Addr_** (IP address of the client making the request), as part of it's set condition. So in my case, I can check if the originating request came from my Varnish server's "192.168.1.150" address, if so set the `is_proxied` environment variable. Afterwards I can use the `is_proxied` environment variable to tell Apache where to log that access request too.

Inside my `VirtualHost` directive, the log configuration looks like this:
```bash
SetEnvIf Remote_Addr "192.168.1.150" is_proxied=1

ErrorLog /var/log/httpd/antoniobaltazar.com/error.log

CustomLog /var/log/httpd/antoniobaltazar.com/access.log cloudflare env=is_proxied
CustomLog /var/log/httpd/antoniobaltazar.com/access-local.log combined
```

Unfortunately, we can't use this same technique to log the error logs separately as <a href="https://httpd.apache.org/docs/2.4/logs.html" target="_blank">ErrorLog</a> does not support this.
