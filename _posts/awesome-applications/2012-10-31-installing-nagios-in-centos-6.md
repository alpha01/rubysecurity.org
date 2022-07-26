---
categories:
  - awesome-applications
  - nagios
tags:
  - nagios
  - centos
layout: post
title: Installing Nagios in CentOS 6
created: 1351722983
---

Make sure you've installed the following packages on your CentOS installation before continuing.

* Apache
* PHP
* GCC compiler
* GD development libraries

```bash
yum install httpd php gcc glibc glibc-common gd gd-devel make autoconf
```

Create `nagios` user and give it a password.

```bash
/usr/sbin/useradd -m nagios
passwd nagios
```

Create a new `nagcmd` group for allowing external commands to be submitted through the web interface. Add both the `nagios` user and the `apache` user to the group. 

```bash
/usr/sbin/groupadd nagcmd
/usr/sbin/usermod -a -G nagcmd nagios
/usr/sbin/usermod -a -G nagcmd apache
```

Download and extract the latest stable Nagios Core from http://www.nagios.org/download/core/
Run the Nagios configure script, passing the name of the group you created earlier: 

```bash
./configure --with-command-group=nagcmd
```

Compile Nagios

```bash
make all
```

Install Nagios

```bash
make install
```

Update the email address associated with the `nagiosadmin` contact definition in `/usr/local/nagios/etc/objects/contacts.cfg`. Install the Nagios web config file in the Apache `conf.d` directory. 

```bash
make install-webconf
```

Install Nagios init config

```bash
make install-init
```

Create htaccess user to access the Nagios web interface.

```bash
htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

Restart Apache to make the new settings take effect. 

```bash
service httpd restart
```

Add Nagios to the list of system services and have it automatically start when the system boots. 

```bash
chkconfig --add nagios
chkconfig nagios on
```

Download and extract the latest stable Nagios Plugins from <a href="http://www.nagios.org/download/plugins/" target="_blank">http://www.nagios.org/download/plugins</a>.
Configure, compile and install the Nagios plugins.

```bash
./configure --with-nagios-user=nagios --with-nagios-group=nagios
make
make install
```

Update permissions

```bash
chown nagios.nagios /usr/local/nagios
chown -R nagios.nagios /usr/local/nagios/libexec
```

Verify the sample Nagios configuration files. 

```bash
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

If there are no errors, start Nagios. 

```bash
service nagios start
```

### Post Installation

To make things easier to myself, the `ncheck` and `nrestart` aliases were created in `/root/.bashrc` to check the nagios configuration and restart the service respectively.

```bash
alias ncheck='/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg'
alias nrestart='service nagios restart'
```

Reference:
* <a href="http://nagios.sourceforge.net/docs/3_0/quickstart-fedora.html" target="_blank">http://nagios.sourceforge.net/docs/3_0/quickstart-fedora.html</a>
