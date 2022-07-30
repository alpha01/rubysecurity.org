---
categories:
  - awesome-applications
  - varnish
tags:
  - php
  - drupal
  - nginx
  - varnish
  - apache
  - security
layout: post
title: Hide Server and Web Application Information
created: 1479796920
---

First and foremost, this is security over obscurity. No relevant security improvement is gained by this, but rather just annoy or hopefully prevent automated bots or script kiddies from doing any future damage. By **NO** means it should be the only defensive mechanism for any site!

This site runs on Drupal. Drupal, by default returns Drupal specific http headers on every request. So I wanted to disabled this completely at the server level, in addition to the PHP and Apache information that is part of a typical stock LAMP configuration.

First, we start with PHP. The PHP `X-Powered-By` header can be disabled by disabling the `expose_php` option:

```bash
expose_php = Off
```

Next, is updating the default `Server` header set by Apache:

```bash
ServerTokens Prod
```

Finally, it's time to remove the `X-Generator` and `X-Drupal-Cache` specific Drupal headers. Using Apache via `mod_headers` module:

{% raw %}
```bash
<IfModule mod_headers.c>
     Header unset X-Generator
     Header unset X-Drupal-Cache
</IfModule>
```
{% endraw %}

Using Nginx via the <a href="https://github.com/openresty/headers-more-nginx-module" target="_blank">headers more</a> module:

```bash
more_clear_headers 'x-generator';
more_clear_headers 'x-drupal-cache';
```

Why stop here when I can set custom headers. So as a joke, I want to tell the world that my sites are powered by Unicors and I’m the hacker being it. Doing so is dead simple.

* Nginx

```nginx
add_header              X-Powered-By "Unicorns";
add_header              X-hacker "Alpha01";
```

* Varnish (vcl_deliver)

```perl
set resp.http.X-Powered-By = "Unicorns";
set resp.http.X-hacker = "Alpha01";
```

### View our changes

Now, let's view my new http headers


```bash
alpha03:~ tony$ curl -I https://www.rubysecurity.org
HTTP/1.1 200 OK
Date: Thu, 24 Nov 2016 03:53:40 GMT
Content-Type: text/html; charset=utf-8
Connection: keep-alive
Set-Cookie: __cfduid=d8fbf8e2a27fac74e782224db3fd3c86c1479959620; expires=Fri, 24-Nov-17 03:53:40 GMT; path=/; domain=.rubysecurity.org; HttpOnly
Strict-Transport-Security: max-age=63072000; includeSubdomains; preload
Expires: Sun, 19 Nov 1978 05:00:00 GMT
Cache-Control: public, max-age=300
X-Content-Type-Options: nosniff
Content-Language: en
X-Frame-Options: SAMEORIGIN
Last-Modified: Tue, 22 Nov 2016 06:55:27 GMT
Vary: Cookie,Accept-Encoding
Front-End-Https: on
X-Powered-By: Unicorns
X-hacker: Alpha01
Server: cloudflare-nginx
CF-RAY: 3069ea499a6320ba-LAX
```

References:

* <a href="http://php.net/manual/en/ini.core.php#ini.expose-php" target="_blank">http://php.net/manual/en/ini.core.php#ini.expose-php</a>
* <a href="http://httpd.apache.org/docs/2.4/mod/core.html#servertokens" target="_blank">http://httpd.apache.org/docs/2.4/mod/core.html#servertokens</a>
* <a href="https://www.drupal.org/node/982034#comment-4719282" target="_blank">https://www.drupal.org/node/982034#comment-4719282</a>
* <a href="http://nginx.org/en/docs/http/ngx_http_headers_module.html" target="_blank">http://nginx.org/en/docs/http/ngx_http_headers_module.html</a>

