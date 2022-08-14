---
categories:
  - programming
  - bash
tags: bash
layout: post
title: Monitor your public IP
created: 1322816662
---

My simple yet effective home public IP monitoring script.

```bash
#!/bin/bash

current_ip="YOURIPHERE"
ip=`curl -s ifconfig.me`

if [ "$current_ip" != "$ip" ] && [ $ip != "" ]
then
    echo "New IP detected: $ip" | mail -s "Public IP has been changed" root@rubyninja.org
fi
```
