---
categories:
  - awesome-applications
  - wordpress
tags: wordpress
layout: post
title: Custom WordPress auto update via FTP
created: 1358720929
---

When I originally migrated my blog off GoDaddy, one of the things that stopped functioning was the WordPress auto update feature. Luckily, I was able to easily overcome this using my own custom FTP settings. For its simplicity, I used `vsftpd`.

Install:

```bash
yum install vsftpd
chkconfig vsftpd on
```

Configure `vsftpd` to jail FTP users to their home directory in `/etc/vsftpd/vsftpd.conf`:

```bash
chroot_local_user=YES
```

Restart vftpd:

```bash
/etc/init.d/vsftpd restart
```

Now, I'll create the user that will be used to download and install the WordPress auto updates:

```bash
useradd -d /PATH/TO/WORDPRESS/SITE -G apache -s /sbin/nologin apache_ftp_user
passwd apache_ftp_user
```

Before applying an update, update your permissions:

```bash
chown -R apache_ftp_user:apache /PATH/TO/WORDPRESS/SITE 
```

Now use `apache_ftp_user` username and password on the WordPress FTP connection wizard page:

<img src="/assets/awesome-applications/wordpress_ftp.png" alt="WordPress FTP Connection Information" title="WordPress FTP Connection Information" />
