---
categories:
  - programming
  - bash
tags: bash
layout: post
title: Exclude comments and empty lines from file
created: 1590303371
---

Every so often theirs the need to view a configuration (usually a large one), and you want an easy way to exclude all comments and empty lines:

```bash
egrep -v '^(#|$)' your-config-file.cfg
```
