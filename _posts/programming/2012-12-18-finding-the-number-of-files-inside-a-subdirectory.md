---
categories:
  - programming
  - bash
tags: bash
layout: post
title: Finding the number of files inside a subdirectory
created: 1355864137
---

```bash
for i in `ls .`; do echo $i; ls -laR $i| grep -v ^[.dlt] | grep -v ^$ | wc -l; echo "--------------------" ; done
```
