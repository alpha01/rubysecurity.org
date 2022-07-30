---
categories:
  - awesome-applications
  - wordpress
tags:
  - wordpress
layout: post
title: Securing the WordPress Admin Dashboard
created: 1392508515
---

So the primary reason why I wanted to add SSL support to www.rubyninja.org is because I want all my /wp-admin traffic to be served securely.

Configuring WordPress to force the login page and all wp-admin traffic to be served over SSL is simply just a matter of defining the `FORCE_SSL_LOGIN` and `FORCE_SSL_ADMIN` constants in `wp-config.php`.

```php
define( 'FORCE_SSL_LOGIN', true );
define( 'FORCE_SSL_ADMIN', true );
```
