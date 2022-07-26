---
categories:
  - awesome-applications
  - nagios
tags:
  - nagios
  - freebsd
layout: post
title: Installing Nagios Remote Plugin Executor in FreeBSD 9.1
created: 1357094039
---

This also installs the Nagios plugins in addition of `nrpe`. Follow the text-based menu install options. The installer will create and configure the nagios user account, and will install the `naios` and `nrpe` plugins in `/usr/local/libexec/nagios`.

```bash
cd /usr/ports/net-mgmt/nrpe2
make install clean
```

Update permissions.

```bash
chown -R nagios:nagios /usr/local/libexec/nagios
```

Create nrpe config file.

```bash
cd /usr/local/etc
cp nrpe.cfg-sample nrpe.cfg
```

Add the following entry to `/etc/rc.conf`.

```bash
nrpe2_enable="YES"
```

Edit `nrpe.cfg` (Example: 192.168.1.5 is my nagios server)

```bash
allowed_hosts=192.168.1.5
```

Start the `nrpe` daemon.

```bash
/usr/local/etc/rc.d/nrpe2 start
```
