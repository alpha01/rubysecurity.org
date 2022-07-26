---
categories:
  - awesome-applications
  - nagios
tags:
  - nagios
  - solaris
layout: post
title: Installing Nagios Remote Plugin Executor in Solaris 11
created: 1356853311
---

Install gcc

```bash
pkg install pkg://sfe/runtime/gcc pkg://sfe/sfe/developer/gcc
```

Install system headers (not really sure if all listed were necessary):

```bash
pkg install SUNWhea SUNWbinutils SUNWarc SUNWgcc SUNWgccruntime SUNWlibsigsegv SUNWgm4 SUNWgnu-automake-110 SUNWaconf
```

Update your `PATH`:

```bash
PATH=$PATH:/usr/gcc/bin:/usr/sfw/bin:/usr/ccs/bin
export PATH
```

Manually create `nagios` user account, home directory, group, and assigned him a password.

```bash
mkdir -p /usr/local/nagios
useradd -d /usr/local/nagios -m nagios
groupadd nagios
usermod -G nagios nagios
passwd nagios
```

Download, extract, compile and install nrpe.

```bash
wget http://prdownloads.sourceforge.net/sourceforge/nagios/nrpe-2.13.tar.gz
tar -xvf http://prdownloads.sourceforge.net/sourceforge/nagios/nrpe-2.13.tar.gz
cd /opt/nrpe-2.13
./configure --with-nagios-user=nagios --with-nagios-group=nagios
make install
make install-daemon-config
cp src/check_nrpe /usr/local/nagios/libexec
```

Update permissions.

```bash
chown -R nagios:nagios /usr/local/nagios/
```

Add the following entry to `/etc/services`

```bash
nrpe 5666/tcp # NRPE
```

Add the following entry to `/etc/inetd.conf`

```bash
nrpe stream tcp nowait nagios /usr/sbin/tcpd /usr/local/nagios/bin/nrpe -c /usr/local/nagios/etc/nrpe.cfg -i
```

Convert and add the new legacy `inetd` entry to SMF.

```bash
inetconv
inetconv -e
```
