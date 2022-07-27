---
categories:
  - awesome-applications
  - apache
tags: apache
layout: post
title: 'Apache: RedirectMatch'
created: 1363479626
---

For the longest time, I've been using <a href="https://httpd.apache.org/docs/current/mod/mod_rewrite.html" target="_blank">mod_rewrite</a> for any type of URL redirect that requires any sort of pattern matching.

A few days ago I migrated my Gallery web app from https://www.rubysecurity.org/photos to http://photos.antoniobaltazar.com and I learned that the `Redirect` directive from <a href="https://httpd.apache.org/docs/2.4/mod/mod_alias.html" target="_blank">mod_alias</a> also has the `RedirectMatch` directive available, which essentially it's `Redirect` with regular expression support.

I was able to easily setup the simple redirect using `RedirectMatch` instead of using mod_rewrite.

```bash
RedirectMatch 301 ^/photos(/)?$ http://photos.antoniobaltazar.com
RedirectMatch 301 /photos/(.*) http://photos.antoniobaltazar.com/$1
```
