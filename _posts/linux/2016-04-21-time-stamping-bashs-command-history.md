---
categories:
  - linux
  - bash
tags: bash
layout: post
title: Time Stamping Bash's command history
created: 1461208333
---

It seems sharing your dot (.) config files is an act that all cool kids do these days. I won't be sharing my Bash configs, however I will share one cool Bash shell trick of time stamping your command history, I use in all of my systems and servers. This is accomplished using the `HISTTIMEFORMAT` environment variable.  Using standard date format control output syntax, it's fairly easy to customize the command history time stamp to whatever time format output you prefer. Finally, in addition to using a customized `HISTTIMEFORMAT` value, I also add the `HISTSIZE` environment variable. This environment variable lets you override the default command history count to a much larger history count.

```bash
HISTSIZE=10000
HISTTIMEFORMAT="%d/%m/%Y %T "
export HISTTIMEFORMAT
```

### Usage

Sample output:

```bash
 3318  21/04/2016 04:42:20 ls
 3318  21/04/2016 04:43:29 vim .bashrc
 3319  21/04/2016 04:43:37 vim .bash_profile
 3320  21/04/2016 05:03:58 man date
 3321  21/04/2016 05:07:43 history |tail
 3322  21/04/2016 05:07:47 history |tail -5
```
