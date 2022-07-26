---
categories:
  - awesome-applications
  - nagios
tags:
  - bash
  - nagios
layout: post
title: 'Writing custom Nagios plugins: check_public-ip'
created: 1357108163
---

Now that I think Nagios is the greatest thing since slice bread, I'm slowly but surely re-writing all my custom monitoring scripts to Nagios plugins.

The following is a Nagios plugin ready script that I used to replace my old public IP monitoring (See <a href="https://www.rubysecurity.org/ip_monitoring" target="_blank">https://www.rubysecurity.org/ip_monitoring</a>).

```bash
#!/bin/bash

STATE_OK=0
STATE_WARNING=1
STATE_CRITICAL=2
STATE_UNKNOWN=3

current_ip="YOUR-IP-ADDRESS-HERE"
ip=`curl -connect-timeout 30 -s ifconfig.me`

if [ "$current_ip" != "$ip" ] || [ -z "$ip" ]
then
        if [[ "$ip" =~ "Service Unavailable" ]] || [[ "$ip" =~ "html" ]]
        then
                echo "IP service monitoring is unavailable."
                exit $STATE_WARNING
        elif [[ "$ip"  =~ [0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3} ]]
        then
                echo "ALERT: Public IP has changed. NEW IP: $ip"
                exit $STATE_CRITICAL
        else
                echo "Unknown state detected."
                exit $STATE_UNKNOWN
        fi

else
        echo "Public OK: $ip"
        exit $STATE_OK
fi
```
