---
categories:
  - programming
  - bash
tags: bash
layout: post
title: Logging your terminal output using script
created: 1423117083
---

I remember when I first discovered the tab key autocomplete in Bash and being absolutely jollied because of it. Having just found the existence of the `script` utility, it feels almost identical.

`script` gives you the capability of logging every thing within your current shell session. In the past, I would always resort to manually copying the text output of my terminal window to a file. In some cases, I would have a really long command line session that I wanted its output saved, which resulted in the entire terminal window crashing when being manually copied due to the extremely large output buffer! Thankfully with `script` those problems are a thing of the past.

### Example

Its usage is dead simple:

```bash
tony@alpha05:~$ script logmyshit.log
Script started, file is logmyshit.log
tony@alpha05:~$ echo "script is fucking awesome!"
script is fucking awesome!
tony@alpha05:~$ exit
```

Contents of logmyshit.log:

```bash
Script started on Wed 04 Feb 2015 10:15:32 PM PST
tony@alpha05:~$ echo "script is fucking awesome!"
script is fucking awesome!
tony@alpha05:~$ exit

Script done on Wed 04 Feb 2015 10:16:00 PM PST

```
