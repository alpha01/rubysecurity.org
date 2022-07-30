---
categories:
  - programming
  - bash
tags:
  - bash
layout: post
title: Spell check from the command line
created: 1430000711
---

I was pleasantly surprise to learn about a utility which lets you spell check text files or any string passed as standard input, directly from the command line. The name of this genius tool is `spell`.

### Examples

Example 1

```bash
tony@alpha05:~$ echo "What the fuc or what the fuck" | spell
fuc
```

Example 2

```bash
tony@alpha05:~$ cat test.txt 
Fuck thi shit.
tony@alpha05:~$ spell test.txt 
thi
```
