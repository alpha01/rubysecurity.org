---
categories:
  - books
  - kubernetes
  - rancher
tags:
  - kubernetes
  - rancher
layout: post
hidden: true
title: Rancher Deep Dive
---

**Rancher Deep Dive: Manage enterprise Kubernetes seamlessly using Rancher** is the book to help you use Rancher and it's associated set of technologies. My history with the Rancher set of Kubernetes technologies is not new. RKE and Rancher have been my go to Kubernetes distribution and management solution that I've been running in my homelab for a few years now. The primary reason being how incredibly easy it was at the time to get a fully functionally, production like, working Kubernetes cluster on my KVM homelab environment, with very minimal effort. Aside from <a href="https://minikube.sigs.k8s.io/docs/start/" target="_blank">Minikube</a> which is really aimed for a quick way to start learning Kubernetes, in my opinion. RKE/2 is still by far the best option to go, when running non-managed Kubernetes.

Whether you're a beginner or advanced Kubernetes user, this book does an excellent job teaching the core Kubernetes concepts and how the Rancher ecosystem comes into place. Each chapter progresses and builds on the knowledge that was described on the previous chapters. You'll learn from the basics of Kubernetes, to deploying a full fledge Kubernetes cluster wether on your own computer with the help of RKE/RKE2 or on the cloud using a managed Kubernetes service. I feel this both Developers and Administrators benefit equally by this book. Obviously, given that Rancher is management solution, they'll be more content in the book regarding administrative tasks such as Kubernetes scaling, Prometheus Monitoring, OPA Gatekeeper Policy constraints, etcd backups, etc.

My only negative critique of this book (and its a massive one!) is that the author doesn't go into detail regarding the different useful features that Rancher includes, that make Kubernetes multi-tenancy easier to implement. This is a massive selling point for the Rancher product, why the author decided to not dedicate a full chapter about this, completely baffles me. Kubernetes authentication can get extremely complex, however even using simple local Rancher user accounts to showcase how Rancher Global Permissions, Cluster, and Project Roles function, would've been sufficient to teach the readers about this awesome feature that we get when using Rancher. In my opinion this would've been much more beneficial than for example, the dedicated chapter for Helm usage and development.

This is a good book, and would highly recommend it anyone wanting to dive into the world of Kubernetes and Rancher.

### Rating: 3/5

Rancher Deep Dive

<a href="https://www.packtpub.com/product/rancher-deep-dive/9781803246093" target="_blank"><img src="/assets/books/Rancher-Deep-Dive.jpg"></a>

* Chapter 1: Introduction to Rancher and Kubernetes
* Chapter 2: Rancher and Kubernetes High-Level Architecture
* Chapter 3: Creating a Single Node Rancher
* Chapter 4: Creating an RKE and RKE2 Cluster
* Chapter 5: Deploying Rancher on a Hosted Kubernetes Cluster
* Chapter 6: Creating an RKE Cluster Using Rancher
* Chapter 7: Deploying a Hosted Cluster with Rancher
* Chapter 8: Importing an Externally Managed Cluster into Rancher
* Chapter 9: Cluster Configuration Backup and Recovery
* Chapter 10: Monitoring and Logging
* Chapter 11: Bringing Storage to Kubernetes Using Longhorn
* Chapter 12: Security and Compliance Using OPA Gatekeeper
* Chapter 13: Scaling in Kubernetes
* Chapter 14: Load Balancer Configuration and SSL Certificates
* Chapter 15: Rancher and Kubernetes Troubleshooting
* Chapter 16: Setting Up a CI/CD Pipeline and Image Registry
* Chapter 17: Creating and Using Helm Charts
* Chapter 18: Resource Management
