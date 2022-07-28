---
categories:
  - awesome-applications
  - nginx
tags:
  - security
  - nginx
layout: post
title: Reverse SSL Proxy with Nginx
created: 1392439824
---

Nginx is turning to be an awesome SSL reverse proxy server, although I can't say I've really put it to real heavy duty use or how it well scale since my sites have relatively slow traffic. Thus said, a reverse SSL proxy using Nginx is working flawless in my environment!

Since all of my sites are being served within a KVM guest using NAT networking, all SSL traffic has to go through the KVM host of which Nginx is being used to proxy the requests to the guest KVM. Nginx is awesome since it supports specifying multiple server blocks (think of virtual hosts in Apache) set to listen on port 443 within the main http block. With this configuration available, it is possible to specify different reverse proxy end points.  

On my server I have enabled SSL for www.rubysecurity.org and www.rubyninja.org.

First thing I needed to do is to map the sites local IPs to the KVM hosts file.

```bash
192.168.100.208 rubysecurity.org www.rubysecurity.org
192.168.100.209 rubyninja.org www.rubyninja.org
```

Then configure `nginx.conf` (sample server blocks):

```nginx
server {
    listen       443;
    server_name  www.rubysecurity.org;
    ssl                 on;
    ssl_certificate     /etc/nginx/certs/www.rubysecurity.org.bundled.crt;
    ssl_certificate_key /etc/nginx/certs/www.rubysecurity.org.key;

    location / {
        proxy_pass   https://www.rubysecurity.org;
	    
        ### Set headers ####
        proxy_set_header        Accept-Encoding   "";
        proxy_set_header        Host            $host;
        proxy_set_header        X-Real-IP       $remote_addr;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;

        #proxy_set_header X-Forwarded-Proto https;##
        #This is better##
        proxy_set_header        X-Forwarded-Proto $scheme;
        add_header              Front-End-Https   on;
 
        # We expect the downsteam servers to redirect to the right hostname, so don't do any rewrites here.
        proxy_redirect     off;
    }
}

server {
    listen   443;
    server_name www.rubyninja.org;
    ssl on;
    ssl_certificate     /etc/nginx/certs/www.rubyninja.org.bundled.crt;
    ssl_certificate_key /etc/nginx/certs/www.rubyninja.org.key;

    location / {
        proxy_pass   https://www.rubyninja.org;

        ### Set headers ####
        proxy_set_header        Accept-Encoding   "";
        proxy_set_header        Host            $host;
        proxy_set_header        X-Real-IP       $remote_addr;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;

        #proxy_set_header X-Forwarded-Proto https;##
        #This is better##
        proxy_set_header        X-Forwarded-Proto $scheme;
        #add_header              Front-End-Https   on;

        # We expect the downsteam servers to redirect to the right hostname, so don't do any rewrites here.
        proxy_redirect     off;
    }
}
```

One interesting thing in Nginx with SSL is that it doesn't have a dedicated Certificate Authority (CA) ssl certificate directive, unlike `SSLCACertificateFile` in Apache. Instead the CA certificate has to be bundled with the public ssl certificate, which it's really not a big deal given that multiple CA's tend to bundle their intermediate CA certificates similarly.   
