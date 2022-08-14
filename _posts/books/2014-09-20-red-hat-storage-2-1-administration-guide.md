---
categories:
  - books
  - glusterfs
tags:
  - glusterfs
  - rhel
layout: post
hidden: true
title: Red Hat Storage 2.1 Administration Guide
created: 1411180628
---

So, a few weeks ago I <a href="https://www.rubysecurity.org/what_is_devops" target="_blank">reviewed</a> a really good article (free epub download) by O'reilly Media that answers the question <a href="http://shop.oreilly.com/product/0636920026822.do" target="_blank">What is DevOps</a>. Now I'm upping the ante, and thus reviewing documentation handbooks. This case it's the Red Hat Storage 2.1 Administration guide, and like what O'reilly Media did with the DevOps article, Red Hat is kindly enough to give out their documentation free of charge as an epub download, which I can easily read it on my freedom hating iPad mini, but I digress..

The last few weeks I've been architecting a High Availability storage solution for our mission critical web applications at work and opted to use GlusterFS. I've always said (and imaged) that in order to any software project to be fully reputable, it needs a good solid book to back it up. To my surprise at the time of the writing of the review their isn't a GlusterFS specific book published. After reading the official Red Hat Storage 2.1 Administration guide is quite clear why their hasn't been a book published for GlusterFS. Reading this documentation is practically all you need to start using GlusterFS and start deploying it on any production environment. Although the documentation explicitly describes implementing GlusterFS on Red Hat Enterprise Linux, I didn't find any problem translating the usage into CentOS. The documentation is not dead boring, but rather actually written like a technical book.  It walks you to installing and configuring GlusterFS on local environment as well as through Amazon Web Services. The only portions that I skimmed trough were the chapters regarding Geo-replication and AWS. Aside that, the documentation covers just about anything you need to know to successfully deploy GlusterFS on production environment. The only portion I feel the documentation needed some additional detail is the NFS High Availability configuration. Red Hat recommends using the Cluster Trivial Database (CTDB) package, which is part of the Samba project, however the documentation doesn't give out any examples on configuring ctdb.

Prior to deploying GlusterFS on a production  environment, I read this documentation along with the official <a href="http://www.gluster.org/wp-content/uploads/2012/05/Gluster_File_System-3.3.0-Administration_Guide-en-US.pdf" target="_blank">GlusterFS documentation</a> (which is practically 80% of the same content covered on the official Red Hat documentation) and quite frankly given how incredibly simple GlusterFS is to implement and manage, I feel that's all the information needed to learning and using GlusterFS for your storage needs.

### Rating: 5/5

Red Hat Storage 2.1 Administration Guide

<a href="https://access.redhat.com/documentation/en-US/Red_Hat_Storage/" target="_blank"><img src="/assets/books/redhat-storage.jpg"></a>

* Chapter 1: Platform Introduction
* Chapter 2: Red Hat Storage Architecture
* Chapter 3: Key Features
* Chapter 4: Use Case Examples
* Chapter 5: Storage Concepts
* Chapter 6: The glusterd Services
* Chapter 7: Trusted Storage Pools
* Chapter 8: Red Hat Storage Volumes
* Chapter 9: Accessing Data – Setting up Clients
* Chapter 10: Managing Red Hat Storage Volumes
* Chapter 11: Configuring Red Hat Storage for Enhancing Performance
* Chapter 12: Managing Geo-replication
* Chapter 13: Managing Directory Quotas
* Chapter 14: Monitoring Your Red Hat Storage Workload
* Chapter 15: Managing Red Hat Storage Volume Life-Cycle Extensions
* Chapter 16: Launching Red Hat Storage Server for Public Cloud
* Chapter 17: Provisioning Storage
* Chapter 18: Stopping and Restarting Red Hat Storage Instance
* Chapter 19: Managing Object Store
* Chapter 20: Troubleshooting
