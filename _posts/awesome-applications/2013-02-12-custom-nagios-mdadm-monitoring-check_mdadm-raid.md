---
categories:
  - awesome-applications
  - nagios
tags:
  - nagios
  - ruby
  - nagios
layout: post
title: 'Custom Nagios mdadm monitoring: check_mdadm-raid'
created: 1360635917
---

Simple Nagios mdadm monitoring plugin.

```ruby
#!/usr/bin/env ruby

# Tony Baltazar, Feb 2012. root[@]rubyninja.org

OK = 0
WARNING = 1
CRITICAL = 2
UNKNOWN = 3

# Note to self, mdadm exit status:
#0 The array is functioning normally.
#1 The array has at least one failed device.
#2 The array has multiple failed devices such that it is unusable.
#4 There was an error while trying to get information about the device.

raid_device = '/dev/md0'

get_raid_output = %x[sudo mdadm --detail #{raid_device}].lines.to_a


get_raid_status = get_raid_output.grep(/\sState\s:\s/).to_s.match(/:\s(.*)\\n\"\]/)
raid_state = get_raid_status[1].strip



if raid_state.empty?
 print "Unable to get RAID status!"
 exit UNKNOWN
end

if /^(clean(, checking)?|active)$/.match(raid_state) 
 print "RAID OK: #{raid_state}"
 exit OK
elsif /degraded/i.match(raid_state)
 print "WARNING RAID: #{raid_state}"
 exit WARNING
elsif /fail/i.match(raid_state)
 print "CRITICAL RAID: #{raid_state}"
 exit CRITICAL
else
 print "UNKNOWN RAID detected: #{raid_state}"
 exit UNKNOWN
end
```
