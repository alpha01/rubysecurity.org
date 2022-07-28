---
categories:
  - macos
  - dns
tags:
  - macos
  - networking
layout: post
title: Flush DNS cache in Mac OS X
created: 1355205267
---
Mountain Lion and Lion

```bash
sudo killall -HUP mDNSResponder
```

Snow Leopard

```bash
sudo dscacheutil -flushcache
```
