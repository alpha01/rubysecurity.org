---
categories:
  - awesome-applications
  - apache
tags:
  - apache
  - centos
layout: post
title: 'Apache - Directory index forbidden by Options directive'
created: 1398616065
---

By default, the CentOS Apache configuration does not allow index directory listings. So I enabled `Indexes Option` on the directory that I wanted allow this feature within my custom vhost. To my surprise after I made the Apache config update, directory listing was not working and I was still getting the default CentOS Apache welcome page.

### Error

```bash
[Sat Apr 26 14:42:11 2014] [error] [client 192.168.100.1] Directory index forbidden by Options directive: /www/mysecureshit/
```

It turns out the default `/etc/httpd/conf.d/welcome.conf` file option overrides the `+Indexing Options` that I explicitly enabled within my custom vhost.

```bash
#
# This configuration file enables the default "Welcome"
# page if there is no default index page present for
# the root URL.  To disable the Welcome page, comment
# out all the lines below.
#
<LocationMatch "^/+$">
    Options -Indexes
    ErrorDocument 403 /error/noindex.html
</LocationMatch>
```

### Fix

Delete  `/etc/httpd/conf.d/welcome.conf`.
