---
categories:
  - awesome-applications
  - mysql
tags: mysql
layout: post
title: Resetting MySQL root password
created: 1324280692
---

1). End current mysql process

2). Run MySQL safe daemon with skipping grant tables

```bash
mysqld_safe --skip-grant-tables &
```

3). Login to MySQL as root with no password:

```bash
mysql -u root mysql
```

4). Run UPDATE query to reset the root password. In MySQL command line prompt issue the following two commands:

```sql
UPDATE user SET password=PASSWORD("NEWPASSWD") WHERE user="root";
FLUSH PRIVILEGES;
```
