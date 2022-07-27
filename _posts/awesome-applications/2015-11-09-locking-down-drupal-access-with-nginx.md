---
categories:
  - awesome-applications
  - nginx
tags:
  - security
  - nginx
  - drupal
layout: post
title: Locking Down Drupal Access with Nginx
created: 1447040450
---

This site is powered by Drupal. Drupal and WordPress for that matter, are well targeted platforms, mainly because of their large install base on the internet. Quite frankly the reason I bother using both Drupal and WordPress instead of a flat-file based CMS is because I have to deal with these web applications at work on a daily basis, so it's a great way to keep myself current with the technology that's paying my bills.

I have Nginx acting as an SSL proxy for www.rubysecurity.org, which is hosted on an Apache back-end. So I have a few configs that I've enabled to lock down access to my Drupal site. The configs are made at the Nginx proxy level, so they can never reach Apache.

Firstly, I have all of Drupal's /admin locked out from outside access:
```nginx
location = /admin {
    allow MY-HOME-IP-ADDRESS;
    deny all;
    return 403;
}
```

Next, I only allow login access from my home ip address:

```nginx
location = /user {
    allow MY-HOME-IP-ADDRESS;
    deny all;
    
    proxy_pass https://www.rubysecurity.org/user;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
}
```

Lastly, since Nginx is unable to process query strings at the location block level, I've setup an additional config to drop all user login query requests.

```nginx
if ($args ~* "q=user") {
    set $blockme  M;
}
if ($remote_addr != MY-HOME-IP-ADDRESS) {
    set $blockme  "${blockme}E";
}
if ($blockme = ME) {
    return 403;
}
```
