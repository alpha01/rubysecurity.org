---
categories:
  - programming
  - perl
tags:
  - perl
  - centos
layout: post
title: Can't locate Time/HiRes.pm CPAN error on CentOS 7
created: 1406509888
---

So the default Perl installation that ships with CentOS 7 minimal install does not include Time::HiRes, which is necessary if you want to use CPAN.

### Error:

```bash
Can't locate Time/HiRes.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 /root) at /usr/share/perl5/Net/Ping.pm line 313.
```

### Fix:

```bash
yum install perl-Time-HiRes
```
