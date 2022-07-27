---
categories:
  - linux
  - php
tags:
  - php
layout: post
title: Installing PHP 5.3 in CentOS
created: 1319696975
---

To view the PHP related packages available in CentOS 5.x, simply run the following command:

```bash
[root@rubyninja ~]# yum search php
```

You should see the PHP 5.3 rpm packages prefixed as `php53`. Now it's just a matter of installing the package, and you should be all set.

```bash
[root@rubyninja ~]# yum install php53 php53-cli
```

**Note**: If you made the mistake of installing PHP 5.1 previously, You will need to remove all of the PHP 5.1 packages currently installed on your system prior to installing the PHP 5.3 packages.
This can be accomplished as the following.

```bash
[root@rubyninja ~]# rpm -qa --queryformat="%{NAME}\n" | grep php
php
php-cli
php-common
php-imap
php-mssql
php-snmp
php-mhash
php-mysql
php-mbstring
[root@rubyninja ~]# yum remove php php-cli php-common php-imap php-mssql php-snmp php-mhash php-mysql php-mbstring
```
