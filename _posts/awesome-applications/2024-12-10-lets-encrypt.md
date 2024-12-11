---
categories:
  - awesome-applications
  - letsencrypt
layout: post
tags:
 - letsencrypt
 - security
title: 
---

I've been using <a href="https://letsencrypt.org/getting-started/" target="_blank">Let's Encrypt</a> for years, and it came to me that I've hardly ever really mentioned this awesome service at all! Let's Encrypt is awesome, plain and simple. I use to throughout my homelab to setup and configure secure access.

Using this awesome is really straight forward. I use the <a href="https://github.com/acmesh-official/acme.sh" target="_blank">acme.sh</a> script for all ssh requests. The <a href="https://github.com/acmesh-official/acme.sh">acme.sh</a> script is simple and works beautifully.

### Setup

I use the <a href="https://github.com/acmesh-official/acme.sh?tab=readme-ov-file#2-or-install-from-git" target="_blank">git repository setup method</a>.

```bash
git clone https://github.com/acmesh-official/acme.sh.git
cd ./acme.sh
./acme.sh --install -m my@example.com
```

The setup process will create a `~/.acme.sh` configuration environment that the script will use to save your Let's Encrypt issued certificates in.

I use the [Automatic DNS API integration](https://github.com/acmesh-official/acme.sh?tab=readme-ov-file#8-automatic-dns-api-integration) approach to verify and issue certificates. For this to work with Cloudflare, I simply just needed to create an <a href="https://developers.cloudflare.com/fundamentals/api/get-started/create-token/" target="_blank">API key</a> and export the following two variables.

```bash
export CF_Key="EXAMPLEKEY"
export CF_Email="my@example.com"
```

Afterwards, it's just a matter of using the <a href="https://github.com/acmesh-official/acme.sh" target="_blank">acme.sh</a> script.

For example:

```bash
./acme.sh --issue --dns dns_cf -d rubyninja.org -d *.antoniobaltazar.com -d *.rubyninja.org -d *.k8s.rubyninja.org -d *.rubysecurity.org
```

The really cool thing is that the script is smart enough to save the environments under `~/.acme.sh/account.conf` for future use (certs are valid for 90 days). In addition it supports wildcards certificates, as well as it being cron friendly!

### Resources

* [https://github.com/acmesh-official/acme.sh](https://github.com/acmesh-official/acme.sh)
* [https://github.com/acmesh-official/acme.sh/wiki/dnsapi#dns_cf](https://github.com/acmesh-official/acme.sh/wiki/dnsapi#dns_cf)
* [https://developers.cloudflare.com/fundamentals/api/get-started/create-token/](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/)
