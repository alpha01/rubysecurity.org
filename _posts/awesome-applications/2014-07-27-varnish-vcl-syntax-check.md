---
categories:
  - awesome-applications
  - varnish
tags: varnish
layout: post
title: Varnish VCL Syntax Check
created: 1406444738
---

```bash
[root@rubyninja varnish]# varnishd -C -f default.vcl 
Message from VCC-compiler:
Expected an action, 'if', '{' or '}'
('input' Line 156 Pos 17)
                erro 403 "Fuck off";
----------------####----------------

Running VCC-compiler failed, exit 1

VCL compilation failed
```
