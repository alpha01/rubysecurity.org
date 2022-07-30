---
categories:
  - awesome-applications
  - bind
tags: bind
layout: post
title: BIND - Typo caused slave zone transfer to stop working
created: 1437107678
---

I was surprised to see a typo had caused all slave transfers to shit themselves. I came across a situation where a new slave zone was specified to a non-existing location in the file system and that caused the rest of the slave zones to get permission denied errors when trying to update.

### Error

```bash
Jul 12 03:23:27 ns2 named[1184]: dumping master file: etc/zones/tmp-Zbk9acg9uv: open: permission denied
Jul 12 03:27:50 ns2 named[1184]: dumping master file: etc/zenos/tmp-4yxBXaUMTq: open: file not found
Jul 12 03:29:46 ns2 named[1184]: dumping master file: etc/zones/tmp-KPqzHa9ev9: open: permission denied
Jul 12 03:38:02 ns2 named[1184]: dumping master file: etc/zones/tmp-kuhtUPjcAi: open: permission denied
```
