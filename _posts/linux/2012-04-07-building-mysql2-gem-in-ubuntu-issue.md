---
categories:
  - linux
  - ruby
tags:
  - ruby
  - mysql
  - ubuntu
layout: post
title: Building mysql2 gem in Ubuntu issue
created: 1333834148
---

### Error

<!-- markdownlint-disable -->
```bash
Installing mysql2 (0.3.11) with native extensions 
Gem::Installer::ExtensionBuildError: ERROR: Failed to build gem native extension.

        /home/tony/.rvm/rubies/ruby-1.9.3-p125/bin/ruby extconf.rb 
checking for rb_thread_blocking_region()... yes
checking for rb_wait_for_single_fd()... yes
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lm... yes
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lz... yes
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lsocket... no
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lnsl... yes
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lmygcc... no
checking for mysql_query() in -lmysqlclient... no
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of
necessary libraries and/or headers.  Check the mkmf.log file for more
details.  You may need configuration options.

Provided configuration options:
	--with-opt-dir
	--with-opt-include
	--without-opt-include=${opt-dir}/include
	--with-opt-lib
	--without-opt-lib=${opt-dir}/lib
	--with-make-prog
	--without-make-prog
	--srcdir=.
	--curdir
	--ruby=/home/tony/.rvm/rubies/ruby-1.9.3-p125/bin/ruby
	--with-mysql-config
	--without-mysql-config
	--with-mysql-dir
	--without-mysql-dir
	--with-mysql-include
	--without-mysql-include=${mysql-dir}/include
	--with-mysql-lib
	--without-mysql-lib=${mysql-dir}/lib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-mlib
	--without-mlib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-zlib
	--without-zlib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-socketlib
	--without-socketlib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-nsllib
	--without-nsllib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-mygcclib
	--without-mygcclib
	--with-mysqlclientlib
	--without-mysqlclientlib
```
<!-- markdownlint-enable -->

### Fix

```bash
sudo apt-get install libmysqld-pic
```
