---
categories:
  - linux
  - networking
tags:
  - ubuntu
  - security
  - freebsd
layout: post
title: OSSEC agent install issue on Debian Squeeze
created: 1365481153
---

So the OSSEC installer has a conditional expression that doesn't seem to be supported with the version of Bash in Debian Squeeze. 

### Error:

```bash
3- Configuring the OSSEC HIDS.

./install.sh: 158: [[: not found
```

## Fix (workaround)

Update line 372 on install.sh to the following:

```bash
if [ "X${USER_AGENT_SERVER_IP}" = "X" -a "X${USER_AGENT_SERVER_NAME}" = "X" ]; then
```

This was so frustrating, and it appears to be known issue. Unless I'm mistaken double brackets are only used in Bash to do a regular expression conditions, at least that's the only time I've used them... 

**_UPDATE_**: I also ran into this same issue on FreeBSD 9 and Ubuntu Server 12.04 LTS.

Resource:
* <a href="https://groups.google.com/forum/?fromgroups=#!topic/ossec-list/jdl8yi5rBgI" target="_blank">https://groups.google.com/forum/?fromgroups=#!topic/ossec-list/jdl8yi5rBgI</a>


