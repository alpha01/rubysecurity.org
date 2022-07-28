---
categories:
  - awesome-applications
  - apache
tags: apache
layout: post
title: Logging mod_rewrite redirects
created: 1324525872
---

Extremely useful for debugging mod_rewrite rules.

```bash
# Trace:
# (!) file gets big quickly, remove in prod environments:
RewriteLog "/web/logs/mywebsite.rewrite.log"
RewriteLogLevel 9
RewriteEngine On
```
