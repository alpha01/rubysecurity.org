---
categories:
  - awesome-applications
  - nagios
tags:
  - nagios
  - centos
  - ubuntu
layout: post
title: Installing Nagios Remote Plugin Executor in CentOS and Ubuntu
created: 1351738882
---

CentOS

```bash
yum install openssl openssl-devel gcc make autoconf xinetd
```

Debian/Ubuntu

```bash
apt-get install openssl build-essential libssl-dev gcc make autoconf xinetd
```

Create `nagios` user and give it a password.

```bash
/usr/sbin/useradd -m nagios
passwd nagios
```

Download and extract the latest stable Nagios Plugins from <a href="http://www.nagios.org/download/plugins/" target="_blank">http://www.nagios.org/download/plugins/</a>
Configure, compile and install the Nagios plugins.

```bash
./configure --with-nagios-user=nagios --with-nagios-group=nagios
make
make install
```

Download the latest NRPE plugin from <a href="http://exchange.nagios.org/directory/Addons/Monitoring-Agents/NRPE--2D-Nagios-Remote-Plugin-Executor/details" target="_blank">http://exchange.nagios.org/directory/Addons/Monitoring-Agents/NRPE--2D-Nagios-Remote-Plugin-Executor/details</a>. Extract, configure, compile and install the plugin with `xinetd` configuration.

```bash
./configure
make all
make install-daemon
make install-xinetd
```

Edit the `/etc/xinetd.d/nrpe` file and add the IP address of the monitoring server to the `only_from` directive.

```bash
only_from = 127.0.0.1 <nagios_ip_address>
```

Add the following entry for the NRPE daemon to the `/etc/services` file.

```bash
nrpe 5666/tcp # NRPE
```

Restart the `xinetd` service.

```bash
/etc/init.d/xinetd restart
```

Copy over sample config file.

```bash
mkdir /usr/local/nagios/etc/
cp sample-config/nrpe.cfg /usr/local/nagios/etc/nrpe.cfg
```

Copy over `check_nrpe` binary to `/usr/local/nagios/libexec`

```bash
cp src/check_nrpe /usr/local/nagios/libexec/
```

Update permissions.

```bash
chown nagios.nagios /usr/local/nagios
chown -R nagios.nagios /usr/local/nagios/libexec
```

Update firewall.

```bash
iptables -A INPUT -p tcp -m tcp --dport 5666 -j ACCEPT
```

### Testing Communication

Issue the following command to test communication on the Nagios monitoring server. Replace IP "192.168.0.1", with the NRPE client's IP.

```bash
/usr/local/nagios/libexec/check_nrpe -H 192.168.0.1
```

You should get a string back that tells you what version of NRPE is installed on the remote host, like this:

```bash
NRPE v2.13
```
