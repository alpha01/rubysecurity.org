---
categories:
  - awesome-applications
  - nagios
tags: nagios
layout: post
title: Nagios SSL Certificate Expiration Check
created: 1541292424
---

So, a while back I demonstrated a way to <a href="https://www.rubysecurity.org/certwatch_Automated-SSL-certificate-expiration-check" target="_blank"> to set up an automated SSL certificate expiration monitoring solution.</a>
Well, it turns out the <a href="https://www.monitoring-plugins.org/doc/man/check_http.html" target="_blank">check_http</a> Nagios plugin has built-in support to monitor SSL certificate expiration as well. This is accomplished using the `-C` / `--certificate` options.

Example check on a local expired Let's Encrypt Certificate:
```bash
[root@monitor plugins]# ./check_http -t 10 -H www.rubysecurity.org -I 192.168.1.61 -C 10
SSL CRITICAL - Certificate 'www.rubysecurity.org' expired on 2018-07-25 18:39 -0700/PDT.
```

`check_http` help doc:

```bash
-C, --certificate=INTEGER[,INTEGER]
    Minimum number of days a certificate has to be valid. Port defaults to 443
    (when this option is used the URL is not checked.)

CHECK CERTIFICATE: check_http -H www.verisign.com -C 30,14

 When the certificate of 'www.verisign.com' is valid for more than 30 days,
 a STATE_OK is returned. When the certificate is still valid, but for less than
 30 days, but more than 14 days, a STATE_WARNING is returned.
 A STATE_CRITICAL will be returned when certificate expires in less than 14 days
```
