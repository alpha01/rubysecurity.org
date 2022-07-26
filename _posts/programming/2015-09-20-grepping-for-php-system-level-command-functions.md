---
categories:
  - programming
  - php
tags:
  - php
  - security
layout: post
title: Grepping for PHP system level command functions
created: 1442775806
---

```bash
 grep --color -r -E -e '(escapeshellarg|escapeshellcmd|exec|passthru|proc_close|proc_get_status|proc_nice|proc_open|proc_terminate|shell_exec|system)(\s+)?\(' ./
```
