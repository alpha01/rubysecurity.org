---
categories:
- programming
- php
layout: post
tags: php
title: PHP 7.4 with Remi's RPM Repository
created: 1616989883
---

Containerizing all my web applications has been on my things to do list for some years now. Until then, I shall continue to run some of my apps in a traditional VM shared environment.

<a href="https://rpms.remirepo.net/" target="_blank">Remi's RPM Repository</a> is the best RPM based repository if you want to easily run the latest upstream version of PHP. One of the benefits of using this repository in a shared environment is the ability to easily run multiple versions of PHP. My sites have been on PHP 7.2 until a few minutes ago. This was because PHP 7.2 is officially deprecated and no longer maintained, so being a good internet citizen I needed to upgrade to the latest PHP 7.4.

Upgrading to PHP 7.4 is extremely easy (assuming your app is not using and legacy functionality that was removed or changed). Since I had PHP 7.2 already running, I simply query for all php72 packages installed on my system, then install their php74 counterpart.

```bash
for package in $(rpm -qa --queryformat "%{NAME}\n"|grep php72 |sed 's/php72/php74/g'); do yum install -y $package; done
```

All of the different PHP configurations can be found under /etc/opt/remi. Once all the packages have been installed, I ported over all my custom PHP ini and fpm settings. In addition I had to change the FPM node pool's default listening node port. For example /etc/opt/remi/php74/php-fpm.d/www.conf

```bash
listen = 127.0.0.1:9002
```

This is to avoid a port collision with the already running PHP-FPM pool that is being used by 7.2

Afterwards, I'm able to start my new PHP 7.4 FPM node pool.

```bash
systemctl enable php74-php-fpm
systemctl start php74-php-fpm
```

The last step, is simply updating my site's Apache configuration to point to the new PHP 7.4  FPM node port.

```bash
<VirtualHost *:80>
    DocumentRoot /www/shit.alpha01.org

    ServerName shit.alpha01.org
    ServerAlias www.shit.alpha01.org

    SetEnvIf Remote_Addr "192.168.1.150" is_proxied=1

    ErrorLog /var/log/httpd/shit.alpha01.org/error.log
    CustomLog /var/log/httpd/shit.alpha01.org/access.log cloudflare env=is_proxied
    CustomLog /var/log/httpd/shit.alpha01.org/access-local.log combined

    ProxyPassMatch ^/(.*\.php)$ fcgi://127.0.0.1:9002/www/shit.alpha01.org/$1
    ProxyTimeout 120
</VirtualHost>
```
