---
categories:
  - programming
  - perl
tags: perl
layout: post
title: Perl - Remove all blank lines from a file
created: 1412386356
---
Remove all blank lines from a file using Perl:

```perl
perl -ne 'print unless /^\s+$/ ' test.txt 
```
