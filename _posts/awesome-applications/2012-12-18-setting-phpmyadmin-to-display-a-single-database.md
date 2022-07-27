---
categories:
  - awesome-applications
  - mysql
tags:
  - php
  - mysql
layout: post
title: Setting phpMyAdmin to display a single database
created: 1355804438
---

Edit `config.inc.php`

```php
$cfg['Servers'][$i]['only_db'] = 'databasename';
```
