---
categories:
  - awesome-applications
  - bind
layout: post
tags: bind
title: Configure BIND to support DDNS updates
created: 1616916115
---

I use <a href="https://www.rubysecurity.org/bind" target="_blank">BIND</a> on my home network for both authoritative and forwarding name resolution. In it I have a few private DNS zones I use for testing and for my homelab setup. The main primary dns zone I use for my homelab is rubyninja.org. Previously when I wanted to make DNS changes, I just ssh into my master nameserver, I update the zone file, and reload. While this worked great for me these last 10+ years that I've running BIND. It obviously doesn't follow good DevOps practices.

If you're in a normal BIND environment where you already using <a href="https://bind.isc.org/doc/arm/9.11/man.rndc.html" target="_blank">rndc</a>, to administer your server, then you're almost quite there.

## BIND Configuration

1). Secret Key Transaction Authentication (TSIG) key. (Where **ddnskey.** is the name of the key)
Approach A: Using dnssec-keygen

```bash
mkdir ddns
dnssec-keygen -a hmac-md5 -b 512 -n HOST -r /dev/urandom ddnskey.
```

The above command will create two Kddnskey files. One ending `*.private` while the other `*.key`.

Approach A: Using tsig-keygen

```bash
tsig-keygen -a hmac-md5 ddnskey.
```

Either approach is fine, for this example I opted to use dnssec-keygen since I'll be using the created key file to test a dynamic dns update.

2). Update named.conf file.
Include the newly created key configuration:

```bash
key "ddnskey." {
        algorithm      "hmac-md5";
        secret          "PRIVATEKEYHERE==";
};
```

Now, it's just a matter of setting the <a href="https://bind9.readthedocs.io/en/v9_16_5/advanced.html#tsig-based-access-control" target="_blank">**allow-update**</a> configuration to allow updates using our newly created key.

```bash
zone "rubyninja.org." IN {
        type master;
        file "etc/zones/db.rubyninja.org";
        allow-transfer { trusted-servers; };
        allow-query { any; };
        allow-update { key rndckey; };
};

zone "k8s.rubyninja.org." IN {
        type master;
        file "etc/zones/db.k8s.rubyninja.org";
        allow-transfer { trusted-servers; };
        allow-query { any; };
        allow-update { key "ddnskey."; };
};
```

It is worth indicating that BIND also includes the <a href="" target="_blank">**update-policy**</a> option for more finer-grained options for the type of updates that we want to allow.  

3). Testing using the tool **dnsupdate** (part of bind-utils) we can easily test doing an update to verify the setup works as expected.

```bash
$  nsupdate -d -k Kddnskey.+157+06602.key
Creating key...
> server ns1.rubyninja.org
> zone k8s.rubyninja.org.
> update add tonytest.k8s.rubyninja.org. 3600 A 192.168.1.25
> send
```

### Resources

* [https://docs.netgate.com/pfsense/en/latest/recipes/bind-rfc2136.html](https://www.thegeekdiary.com/how-to-use-rndc-command-command-line-administration-tool-for-named/)
* [https://www.thegeekdiary.com/how-to-use-rndc-command-command-line-administration-tool-for-named/](https://www.thegeekdiary.com/how-to-use-rndc-command-command-line-administration-tool-for-named/)
