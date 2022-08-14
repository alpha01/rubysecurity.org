---
categories:
  - awesome-applications
  - chef
tags:
  - ubuntu
  - chef
layout: post
title: Chef encountered an error attempting to create the client
created: 1379317250
---

So I'm finally starting to keep up with modern times and started to learn Chef more in depth. My goal is to completely automate and easily manage all of virtual machine instances running in my home network.

Upon attempting to bootstrap my very first node, I received the following error:

```bash
ubuntu Creating a new client identity for ubuntu01 using the validator key.
ubuntu
ubuntu ===================================================================
ubuntu Chef encountered an error attempting to create the client "ubuntu01"
ubuntu ===================================================================
ubuntu
ubuntu
ubuntu Resource Not Found:
ubuntu -------------------
ubuntu The server returned a HTTP 404. This usually indicates that your chef_server_url is incorrect.
ubuntu
ubuntu
ubuntu
ubuntu Relevant Config Settings:
ubuntu -------------------------
ubuntu chef_server_url "https://chef.rubyninja.org:443"
ubuntu
ubuntu
ubuntu
ubuntu [2013-09-15T22:25:28-07:00] FATAL: Stacktrace dumped to /var/chef/cache/chef-stacktrace.out
ubuntu Chef Client failed. 0 resources updated
ubuntu [2013-09-15T22:25:28-07:00] FATAL: Chef::Exceptions::ChildConvergeError: Chef run process exited unsuccessfully (exit code 1)
```

This essentially means that the node is not able to communicate with the Chef server. In my case, it turned out that the ubuntu01 machine was not using my local DNS servers, thus the `chef.rubyninja.org` lookup from the machine was failing.
