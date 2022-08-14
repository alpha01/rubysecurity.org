---
categories:
  - awesome-applications
  - nagios
tags:
  - perl
  - nagios
  - networking
layout: post
title: Monitoring TFTPd server
created: 1379494609
---

So I just spent the last two hours of my life trying to figure why PXE booting was not working in my home network. Turned out the root cause was my fault completely since, I forgot to add a firewall rule on my dhcp/PXE server to allow incoming UDP connections on port 69.

### Fix

```bash
iptables -A INPUT -p udp -m udp --dport 69 -j ACCEPT
```

As with just about any other service, this service can be monitored using Nagios. Originally, I had problems using the `check_tftp.pl` and `check_tftp` plugins that are available from on Nagios Exchange repo, mainly because of the way I have setup my machines.

* `check_tftp` - This plugin was useless in my environment because this plugin all it does, is send out an _status_ command to the TFTP server. Since I'm using the BSD tftp client, all status commands sent to any host will always show up as being connected regardless. <a href="http://exchange.nagios.org/directory/Plugins/Network-Protocols/TFTP/check_tftp/details" target="_blank">http://exchange.nagios.org/directory/Plugins/Network-Protocols/TFTP/check_tftp/details</a>

* `check_tftp.pl` - This plugin was not opted to work in my environment. Mainly because it uses `Net::TFTP`, unlike the tftp client application, `Net::TFTP` does not support specifying a custom reverse connection port (or port ranges). By default, when connecting to a TFTP server, the TFTP server will dynamically choose a random non-standard port to connect back to the client machine and proceed with the TFTP download. My Nagios machine (like all of my machines) are set to drop all incoming packets except for specific ports and related/established connections. <a href="http://exchange.nagios.org/directory/Plugins/Network-Protocols/TFTP/check_tftp-2Epl/details" target="_blank">http://exchange.nagios.org/directory/Plugins/Network-Protocols/TFTP/check_tftp-2Epl/details</a>

This lead me to the path of writing my own custom solutions. So I wrote a simple Nagios plugin that monitors TFTP. All it simply does, is download a non-empty file called test.txt.

<!-- markdownlint-disable -->
```perl
#!/usr/bin/perl -w
# Tony Baltazar. root[@]rubyninja.org

use strict;
use Getopt::Long;

my %options;
GetOptions(\%options, "host|H:s", "port|p:i", "rport|R:s", "file|f:s", "help");

if ($options{help}) {
	usage();
	exit 0;
} elsif ($options{host} && $options{port} && $options{file}) {
	chdir('/tmp');

	my $cmd_str = ( $options{rport} ?  "/usr/bin/tftp -R $options{rport}:$options{rport} $options{host} $options{port} -c get $options{file}" : "/usr/bin/tftp $options{host} $options{port} -c get $options{file}");

	my $cmd = `$cmd_str`;
	if ($? != 0) {
		print "CRITICAL: $cmd";
		system("rm -f /tmp/$options{file}");
		exit 2;
	} else {
		if (! -z "/tmp/$options{file}" ) {
			print "TFTP is ok.\n$cmd";
			system("rm -f /tmp/$options{file}");
			exit 0;
		} else {
			print "WARNING: $cmd";
			system("rm -f /tmp/$options{file}");
			exit 1;
		}
	}

} else {
	usage();
}


sub usage {
print <<EOF;

$0: TFTP monitor check Nagios plugin.

Syntax: $0 [--help|-H=<TFTP server> --port=<TFTP Port> --file=<Test file>]

   --host | -H  : TFTP server.
   --port | -p  : TFTP Port.
   --file | -m  : Test file that will be downloaded.
   --help | -h  : This help message.

Optionally,
   --rport | -R : Explicitly force the reverse originating connection's port.

EOF
}
```
<!-- markdownlint-enable -->

<a href="https://github.com/alpha01/SysAdmin-Scripts/blob/master/nagios-plugins/check_tftp.pl" target="_blank">check_tftp.pl</a>

### Seeing the plugin in action

Assuming, we're using `port udp 1069` to allow the TFTP server (192.168.1.2) to connect to the Nagios monitoring machine.

```bash
[root@monitor libexec]# iptables -L -n |grep "Chain INPUT"
Chain INPUT (policy DROP)
[root@monitor libexec]# iptables-save|grep 1069
-A INPUT -s 192.168.1.2/32 -p udp -m udp --dport 1069 -j ACCEPT
```

Firewall not allowing TFTP to connect back using port 1066.

```bash
[root@monitor libexec]#  su - nagios -c '/usr/local/nagios/libexec/check_tftp.pl -H 192.168.1.2 -p 69 -R 1066 -f test.txt'
CRITICAL: Transfer timed out.
```

Downloading a non-existing file from the TFTP server.

```bash
[root@monitor tmp]#  su - nagios -c '/usr/local/nagios/libexec/check_tftp.pl -H 192.168.1.2 -p 69 -R 1069 -f test.txtFAKESHIT'
WARNING: Error code 1: File not found
```

Successful connection and transfer.

```bash
[root@monitor tmp]#  su - nagios -c '/usr/local/nagios/libexec/check_tftp.pl -H 192.168.1.2 -p 69 -R 1069 -f test.txt'
TFTP is ok.
```
