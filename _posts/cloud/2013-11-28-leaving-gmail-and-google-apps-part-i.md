---
categories:
  - cloud
  - google
tags:
  - google
  - centos
  - kvm
  - varnish
  - nginx
layout: post
title: 'Leaving Gmail and Google Apps: Part I'
created: 1385615066
---

Since I'm paying for essentially unmanaged dedicated hosting so I can run my mail server. I opted to consolidate my personal web applications to the same physical box. This is why I created a KVM guest that would solely be used for my web traffic. One of the main challenges I'm faced is the fact that I only have one public IP address. This means that all of my KVM guests have been configured using the default NAT networking.

For all http traffic I'm using Varnish as the proxy and caching server and for https traffic I'm using Nginx.
<img src="/assets/cloud/http_architecture.png" alt="http_architecture"  title="HTTP and HTTPS Architecture">

First thing that broke using this different architecture on my sites are the `mod_access` IP restrictions that I originally had set in place previously. This is because the Apache backend see's all requests originating from the Varnish and Nginx proxies. Luckily both Varnish and Nginx have really simple access control mechanism built in them.

For example, in Varnish I can create a list of IPs that I can use to their block or grant access to certain URLs.

```perl
acl admin {
   "localhost";
    "MyPublicIPAddress";
} 

sub vcl_recv {
  # Only allow access to the admin ACL
  if (req.url ~ "^/secureshit" && req.http.host ~ "rubysecurity.org") {
    if (client.ip ~ admin) {
      return(pass);
    } else {
      error 403 "Not allowed in admin area.";
    }
  }
}
```

Nginx acl

```nginx
location  /secureshit {
  allow MyPublicIPAddres;
  deny all;
  proxy_pass https://www.rubysecurity.org/secureshit;
}
```
