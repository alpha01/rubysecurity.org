---
categories:
  - awesome-applications
  - nagios
tags: nagios
layout: post
title: Ah Shit - check_http string
created: 1428898699
---

After updating the themes of <a href="http://www.alpha01.org" target="_blank">www.alpha01.org</a>, <a href="http://www.rubysecurity.org" target="_blank">www.rubysecurity.org</a>, <a href="http://www.rubyninja.org" target="_blank">www.rubyninja.org</a> I completely forgot to also update the header template files to include once again their respective Google Analytics tracking code. This resulting in almost three months of no stats.  When I originally setup the Nagios check_http 's on my sites, I didn't set them to also search for the custom Google Analytics string, which I always use this configuration at work on all http checks. 

This can easily be accomplish using the `-s`|`--string` option of the `check_http` plugin.

```bash
/usr/local/nagios/libexec/check_http -I www.rubysecurity.org -S -t 10 --string UA-12912270-3
```

So the lesson learned, you should always configure your `check_http` Nagios service checks to also search for a custom string as part of the check!
