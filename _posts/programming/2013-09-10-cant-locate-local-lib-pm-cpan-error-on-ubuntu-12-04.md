---
categories:
  - programming
  - perl
tags:
  - perl
  - ubuntu
layout: post
title: Can't locate local/lib.pm CPAN error on Ubuntu 12.04
created: 1378788645
---

So the default Perl installation that ships with Ubuntu 12.04 LTS, does not include `local::lib` which is necessary if you want to use CPAN.

### Error:

```bash
Can't locate local/lib.pm in @INC (@INC contains: /home/tony/perl5/lib/perl5 /etc/perl /usr/local/lib/perl/5.14.2 /usr/local/share/perl/5.14.2 /usr/lib/perl5 /usr/share/perl5 /usr/lib/perl/5.14 /usr/share/perl/5.14 /usr/local/lib/site_perl /home/tony) at /usr/share/perl/5.14/CPAN/FirstTime.pm line 1300.
```

### Fix

```bash
sudo apt-get install liblocal-lib-perl
```

Reference:

* <a href="http://stackoverflow.com/questions/16702642/cant-locate-local-lib-pm-in-inc-at-usr-share-perl-5-14-cpan-firsttime-pm" target="_blank">http://stackoverflow.com/questions/16702642/cant-locate-local-lib-pm-in-inc-at-usr-share-perl-5-14-cpan-firsttime-pm</a>
