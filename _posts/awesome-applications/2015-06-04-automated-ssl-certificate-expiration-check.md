---
categories:
  - awesome-applications
  - centos
tags:
  - centos
  - monitoring
layout: post
title: Automated SSL certificate expiration check
created: 1433391163
---

It is quite simple to automate checking for near expiring SSL certificates in CentOS. This is accomplished using the `certwatch` tool. This tool is part of the `crypto-utils` package.

```bash
yum install crypto-utils
``

Installing `crypto-utils`, will create the following cron job, `/etc/cron.daily/certwatch`. By default the `/etc/cron.daily/certwatch` script only checks for SSL certificates loaded by Apache (`httpd -t -DDUMP_CERTS`). So Apache users don't have to do any additional config changes to in order to  automate the check of near expiring SSL certificates.

Since in https://www.rubysecurity.org I use Nginx as a SSL termination proxy for an Apache backend webapp on a different machine. I had to manually update the `/etc/cron.daily/certwatch` script to point to my SSL certificates directly.

```bash
#certs=`${httpd} ${OPTIONS} -t -DDUMP_CERTS 2>/dev/null | /bin/sort -u`
INCLUDE_CERTS='/etc/nginx/certs/*.crt'
certs=`ls $INCLUDE_CERTS 2>/dev/null`
```

Here is an example of an expired SSL certificate alert

```bash
[root@rubyninja certs]# certwatch /etc/nginx/certs/www.rubysecurity.org_2014/www.rubysecurity.org.crt
To: root
Subject: The certificate for www.rubysecurity.org has expired

 ################# SSL Certificate Warning ################

  Certificate for hostname 'www.rubysecurity.org', in file (or by nickname):
     /etc/nginx/certs/www.rubysecurity.org_2014/www.rubysecurity.org.crt

  The certificate needs to be renewed; this can be done
  using the 'genkey' program.

  Browsers will not be able to correctly connect to this
  web site using SSL until the certificate is renewed.

 ##########################################################
                                  Generated by certwatch(1)
```

`certwatch` is far from perfect. It doesn't have any verbose output when doing a check, it solely relies on its exit status to verify if the check was successful. Excerpt from the man page is somewthat appalling.

```bash
DIAGNOSTICS
       The exit code indicates the state of the certificate:

       0
           The certificate is outside its validity period, or approaching expiry

       1
           The certificate is inside its validity period, or could not be parsed
```
