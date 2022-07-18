---
categories:
- awesome-applications
- 389-directoryserver
layout: post
tags: 389-directoryserver
title: Deploying a 389 Directory Server
created: 1616903134

---
So it's been roughly nine months since I created a useful technical post on this site. So what better way to than to post the information about the newly deployed <a href="https://directory.fedoraproject.org/index.html" target="_blank">LDAP 389 Directory Server</a> I just did on my homelab.

Ever since <a href="https://developers.redhat.com/articles/faqs-no-cost-red-hat-enterprise-linux" target="_blank">Red Hat announced that RHEL was going to be of no cost for developer and testing personal use</a> (with limits, of course). This was perfect occasion for me to start using RHEL 8.

## Install

1). Disable SELinux (yes, I know. I should do better..)
```bash
sudo setenforce 0
```

2). Update firewall
```bash
firewall-cmd --permanent --add-port={389/tcp,636/tcp,9830/tcp}
firewall-cmd --reload
firewall-cmd --list-all 
```

3). Install epel repo
```bash
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
yum module install 389-directory-server:stable/default
```

4). Create LDAP instance

```bash
[general]
config_version = 2

[slapd]
root_password = MY_SUPER_ULTRA_SECURE_PASSWORD_HERE

[backend-userroot]
sample_entries = yes
suffix = dc=rubyninja,dc=org
```

5). Create 389 DS instance
```bash
dscreate from-file nstance.inf
```

6). Create ~/.dsrc config

```bash
[localhost]
# Note that '/' is replaced to '%%2f'.
uri = ldapi://%%2fvar%%2frun%%2fslapd-localhost.socket
basedn = dc=rubyninja,dc=org
binddn = cn=Directory Manager
```

7). Afterwards, I'm able to verify my installation
```bash
[root@ldap ldap]# dsctl localhost status
Instance "localhost" is running
```

8). Since, I kept the default settings when I created the  Create 389 DS instance, my server received the name "localhost". Hence why my **~/.dsrc** config also has the instance configured as "localhost". The corresponding systemd service and **dirsrv@localhost** and with the config files stored in /etc/dirsrv/slapd-localhost
```bash
systemctl status dirsrv@localhost

ls -l /etc/dirsrv/slapd-localhost/
```

## SSL Configuration

By default the ds-389 setup is using self-sign certificates. The following was used to install my self-sign cert for ldap.rubyninja.org.

1). Create private root CA key ssh signed cert

```bash
openssl genrsa -out rootCA.key 4096
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 4096 -out rootCA.pem
```

2). I created the following script to easily generate a certificate key-pair signed by my custom local CA.
```bash
#!/bin/bash
SAN="DNS:ldap.rubyninja.org,DNS:login.rubyninja.org"

[[ ! -d "./certs" ]] && mkdir certs

cat \
/etc/pki/tls/openssl.cnf \
- \
<<-CONFIG > certs/ca-selfsign-ssl.cnf

[ san ]
subjectAltName="${SAN:-root@localhost.localdomain}"
CONFIG

# generate client key
openssl genrsa -out certs/ssl.key 4096

# generate csr
openssl req \
-sha256 \
-new \
-key certs/ssl.key \
-reqexts san \
-extensions san \
-subj "/CN=ldap.rubyninja.org" \
-config certs/ca-selfsign-ssl.cnf \
-out certs/ssl.csr


# sign cert
openssl x509 -req -in certs/ssl.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -days 2048 -sha256 -extensions san -extfile certs/ca-selfsign-ssl.cnf -out certs/ssl.crt
```

3). Then I had to **certutil utility to view the names and attributes the default SSL certs had. 

```bash
[root@ldap]# certutil -L -d /etc/dirsrv/slapd-localhost/ -f /etc/dirsrv/slapd-localhost/pwdfile.txt

Certificate Nickname                                         Trust Attributes
                                                             SSL,S/MIME,JAR/XPI

ca_cert                                                      CT,,
Server-Cert                                                  u,u,u
```

4). Once I made note of the name and attributes of the SSL certificates, we will first need to delete them before replacing them with my custom SSL certs. Deletion:

```bash
certutil -d /etc/dirsrv/slapd-localhost/ -n Server-Cert -f /etc/dirsrv/slapd-localhost/pwdfile.txt -D Server-Cert.crt
certutil -d /etc/dirsrv/slapd-localhost/ -n Self-Signed-CA -f /etc/dirsrv/slapd-localhost/pwdfile.txt -D Self-Signed-CA.pem
</blockquote>
Adding new SSL certs:
<blockquote>
certutil -A -d /etc/dirsrv/slapd-localhost/ -n "ca_cert" -t "CT,," -i rootCA.pem -f /etc/dirsrv/slapd-localhost/pwdfile.txt
certutil -A -d /etc/dirsrv/slapd-localhost/ -n "Server-Cert" -t ",," -i ssl/ssl.crt -f /etc/dirsrv/slapd-localhost/pwdfile.txt
```

5). While the **certutil** utility manages signed public and CA certificates. Private SSL certificates are managed by the **pk12util** utility. However, before we use this tool, we must covert the X.509 private ssl certificate to a pkcs12 format.

```bash
openssl pkcs12 -export -out certs/ssl.pfx -inkey certs/ssl.key -in certs/ssl.crt -certfile /root/ssl/rootCA.pem
```

6). Afterwards, we can added it to our LDAP SSL database.

```bash
pk12util -d /etc/dirsrv/slapd-localhost/ -i certs/ssl.pfx
```

7). Lastly, restart the service

```bash
systemctl restart dirsrv@localhost
```

### Resources:

* <a href="https://directory.fedoraproject.org/docs/389ds/howto/quickstart.html#setup-the-instance" target="blank">https://directory.fedoraproject.org/docs/389ds/howto/quickstart.html#setup-the-instance</a>
* <a href="https://directory.fedoraproject.org/docs/389ds/howto/howto-install-389.html" target="_blank">https://directory.fedoraproject.org/docs/389ds/howto/howto-install-389.html</a>
* <a href="https://directory.fedoraproject.org/docs/389ds/howto/howto-ssl-archive.html" target="_blank">https://directory.fedoraproject.org/docs/389ds/howto/howto-ssl-archive.html</a>
* <a href="https://support.globalsign.com/ssl/ssl-certificates-installation/converting-certificates-openssl" target="_blank">https://support.globalsign.com/ssl/ssl-certificates-installation/converting-certificates-openssl</a>
