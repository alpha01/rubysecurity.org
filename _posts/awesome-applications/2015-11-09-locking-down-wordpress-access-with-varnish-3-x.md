---
categories:
  - awesome-applications
  - varnish
tags:
  - security
  - varnish
  - nginx
  - wordpress
layout: post
title: Locking Down WordPress Access with Varnish 3.x
created: 1447042696
---

I have Varnish in front of all my WordPress sites and configured all /wp-admin traffic use https via Nginx. See <a href="https://www.rubysecurity.org/wordpress_admin-ssl" target="_blank">https://www.rubysecurity.org/wordpress_admin-ssl</a>

So to lock down access to my WordPress site's requires both Varnish and Nginx configs to be modified.

Block at the http Varnish level:

```perl
sub vcl_recv {
    if ((req.url ~ "wp-(login|admin)") && (client.ip !~ MY-IP-ADDRESS)) {
                error 403 "Fuck off";
        }
}
```

Block at the https Nginx level (using shit.alpha01.org as an example):

```nginx
location /wp-admin {
        allow   MY-IP-ADDRESS;
        deny all;
        proxy_pass https://shit.alpha01.org/wp-admin;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
}
location /wp-login.php {
        allow MY-IP-ADDRESS;
        deny all;
        proxy_pass https://shit.alpha01.org/wp-login.php;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
}
```
