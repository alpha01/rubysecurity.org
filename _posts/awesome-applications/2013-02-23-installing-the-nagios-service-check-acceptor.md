---
categories:
  - awesome-applications
  - nagios
tags:
  - nagios
  - centos
layout: post
title: Installing the Nagios Service Check Acceptor
created: 1361591868
---

One of the cool things that Nagios supports is the ability to do passive checks. That is instead of Nagios actively checking a client machine for errors, the client is able to send error notifications to Nagios. This can be accomplished using the Nagios Service Check Acceptor.

Installing plugin is a straight forward process. The following steps were the ones I made to get it working under CentOS 6 (Nagios server) and CentOS 5 (client).

Install dependencies:

```bash
yum install libmcrypt libmcrypt-devel
```

Download latest stable version (I tend to stick with stable versions, unless it's absolutely necessary to run development versions), configure and compile.

```bash
wget http://prdownloads.sourceforge.net/sourceforge/nagios/nsca-2.7.2.tar.gz
tar -xvf nsca-2.7.2.tar.gz
cd nsca-2.7.2
./configure
[...]
*** Configuration summary for nsca 2.7.2 07-03-2007 ***:

 General Options:
 -------------------------
 NSCA port:  5667
 NSCA user:  nagios
 NSCA group: nagios

make all
</blockquote>

Copy xinet.d sample config file and nsca.cfg file.
<blockquote>
cp sample-config/nsca.cfg /usr/local/nagios/etc/
cp sample-config/nsca.xinetd /etc/xinetd.d/nsca
```

 Update `/etc/xinetd.d/nsca.xinetd/nsca` (where 10.10.1.20 is the client IP that will be passively checked)

```bash
# default: on
	# description: NSCA
	service nsca
	{
        	flags           = REUSE
	        socket_type     = stream        
        	wait            = no
	        user            = nagios
		group		= nagcmd
        	server          = /usr/local/nagios/bin/nsca
	        server_args     = -c /usr/local/nagios/etc/nsca.cfg --inetd
        	log_on_failure  += USERID
	        disable         = no
		only_from       = 10.10.1.20
	}
```

Restart `xinet.d` 

```bash
service xinetd restart
```

Verify xinetd it's running

```bash
netstat -anp|grep 5667
tcp        0      0 :::5667                     :::*                        LISTEN      30008/xinetd 
```

Add firewall rule

```bash
iptables -A INPUT -p tcp -m tcp --dport 5667 -s 10.10.1.20 -j ACCEPT
```

Finally, set password and update decryption type in `/usr/local/nagios/etc/nsca.cfg`, and update the permissions so no one can read the password.

```bash
chmod 400 /usr/local/nagios/etc/nsca.cfg
chown nagios.nagios /usr/local/nagios/etc/nsca.cfg
```


Now lets configure the client machine. The same dependencies also need to be installed on the client system. I also went ahead and download and compiled nsca. (In theory I could of just copied over the send_nsca binary that was compiled on the Nagios server since both are x64 Linux systems).
Once compiled, copy the send_nsca binary and update its permissions.

```bash
cp src/send_nsca /usr/local/nagios/bin/
chown nagios.nagios /usr/local/nagios/bin/send_nsca
chmod 4710 /usr/local/nagios/bin/send_nsca
```

Copy the sample send_nsca.cfg config file and update the encryption settings, this must match those as the nsca server

```bash
cp sample-config/send_nsca.cfg /usr/local/nagios/etc/
```

Finally, update the permissions so no one can read the password. 

```bash
chown nagios.nagios /usr/local/nagios/etc/send_nsca.cfg 
chmod 400 /usr/local/nagios/etc/send_nsca.cfg 
```

Now you can use the following test script to test the settings.

```bash
#!/bin/bash
CFG="/usr/local/nagios/etc/send_nsca.cfg"
CMD="rubyninja;test;3;UNKNOWN - just an nsca test"
 
/bin/echo $CMD| /usr/local/nagios/bin/send_nsca -H $nagiosserveriphere -d ';' -c $CFG
```

In my case:

```bash
[root@rubyninja ~]# su - nagios -c 'bash /usr/local/nagios/libexec/test_nsca'
1 data packet(s) sent to host successfully.
</blockquote>

Server successfully received the passive check.
<pre class="brush: plain">
Feb 22 20:46:39 monitor nagios: Warning:  Passive check result was received for service 'test' on host 'rubyninja', but the service could not be found!
```

Last words, the only problem I ran into was having xinetd load the newly available nsca properly.

```bash
xinetd[3499]: Started working: 0 available services
nsca[3615]: Handling the connection...
nsca[3615]: Could not send init packet to client
```

**Fix:** This was because the sample `nsca.xinetd` file had the nagios as the group setting. I simply had to update it to `nagcmd`. I suspect this is because of the permissions set on the Nagios command file nagios.cmd, which is the interface for the external commands sent to the Nagios server.
