---
categories:
  - kubernetes
  - rancher
  - keycloak
tags:
  - kubernetes
  - rancher
  - keycloak
  - security
layout: post
title: Deploying Keycloak Identity Provider (IdP) for secure Rancher User Authentication Part 1
---

No words can explain the constant headaches I've gotten throughout my career when working with LDAP in the Unix/Linux world. While I've had plenty of experience working with it in the past ([https://www.rubysecurity.org/tag/ldap](https://www.rubysecurity.org/tag/ldap)), it's certainly not the easiest or pleasant thing to work with. So I'm glad to see new (to me at least) Identity Provider (IdP) tools like <a href="https://www.keycloak.org/" target="_blank">Keycloak</a> that can help us manage user identities, authentication, and access control across applications and systems in a much easier fashion. But more importantly supporting integrations with authentication mechanisms like OpenID Connect (OIDC), OAuth2, SAML to name a few.

### Environment Setup

To get up and running quickly, I opted to deploy Keycloak on a Ubuntu 24.04 VM instead of the container/kubernetes approach.

1). Install required packages.

```bash
apt install openjdk-21-jre unzip
```

2). Download and extract keycloak.

```bash
cd /opt
wget https://github.com/keycloak/keycloak/releases/download/26.0.5/keycloak-26.0.5.zip
unzip keycloak-26.0.5.zip
ln -s keycloak-26.0.5 keycloak
```

3). Setup SSL certificates. At this stage, I had manually issued an Let's Encrypt SSL certificate for `sso.rubyninja.org` for Keycloak. and copied it over to `/opt/keycloak/conf/certs`

4). Update the following settings on `/opt/keycloak/conf/keycloak.conf`.

```bash
# Hostname for the Keycloak server.
hostname=sso.rubyninja.org

# The file path to a private key in PEM format.
https-certificate-key-file=/opt/keycloak/conf/certs/MYKEY.key

# The file path to a server certificate or certificate chain in PEM format.
https-certificate-file=/opt/keycloak/conf/certs/MYCERT.crt
```

5).  Create initial bootstrap admin username/password

```bash
/opt/keycloak/bin/kc.sh bootstrap-admin user
Enter username [temp-admin]:temp-admin
Enter password:
Enter password again:
```

5). Start up the application

```bash
screen -dm /opt/keycloak/bin/kc.sh start --verbose
```

After login in with the temp-admin account, I had to manually create a separate admin user.

By no means this is a production ready setup, but for a homelab environment for testing, this setup is more than sufficient for me.

### Resources

* [https://www.keycloak.org/getting-started/getting-started-zip](https://www.keycloak.org/getting-started/getting-started-zip)
