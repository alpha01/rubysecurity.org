---
categories:
  - "awesome-applications"
  - 389-directoryserver
layout: post
tags:
  - 389-directoryserver
  - rhel
  - cockpit
title: 389 Directory Server GUI with Cockpit
created: 1616911836
---

So I have a 389 Directory Server up and running. The next step to ease administration was to find a GUI. My first logical approach was to use <a href="https://directory.apache.org/studio/" target="_blank">Apache Studio</a>, however I'm trying to the keep number of non ARM applications on my shiny Apple M1 MacBook Pro to an absolute minimum, so I opted to not install Apache Studio. At least not yet. Luckily, I learned that RedHat has a <a href="https://www.webmin.com/" target="_blank">Webmin</a> equivalent called <a href="https://cockpit-project.org/" target="_blank">Cockpit</a> that comes with built-in support for 389 Directory Server Management.  

The Cockpit application was already installed on my base RHEL 8 system, I simply just copied over my fullchain Let's Encrypt SSL certificate to `/etc/cockpit/ws-certs.d/ssl.cert` and restart the service.

```bash
systemctl restart cockpit
```

Then it was just a matter of updating firewalld

```bash
firewall-cmd --add-port=9090/tcp
firewall-cmd --permanent --add-port=9090/tcp
firewall-cmd --reload
```

The Cockpit application runs by default on port `:9090`

<img src="/assets/awesome-applications/cockpit.png"  alt="Cockpit web interface" width="800" height="600"/>
