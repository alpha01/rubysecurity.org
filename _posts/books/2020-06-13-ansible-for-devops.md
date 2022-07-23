---
categories:
  - books
  - ansible
tags: ansible
layout: post
title: Ansible for DevOps
created: 1592022758
---

Ansible for DevOps is a fantastic intermediate to advanced Ansible book. I haven’t read many Ansible books, but given the content covered here, I’d say this is the only book that you really need to read if you want to be proficient with Ansible.

I’ve been using Ansible almost since it’s inception, and although technically this book is aimed for new Ansible users. I didn’t felt this book was dumbed down whatsoever.  <a href="https://www.jeffgeerling.com/" target="_blank">Jeff Geerling</a>  is a prominent figure in the Ansible community, and I have reference many of his blog postings multiple times. So odds are, if you stumble into an Ansible problem, then you might end up Googling your way into one of his articles, and would help you fix your problem! Another cool thing is that the author is also doing weekly hour plus <a href="https://www.youtube.com/watch?v=goclfp6a2IQ&list=PL2_OBreMn7FqZkvMYt6ATmgC0KAGGJNAN
" target="_blank">YouTube stream</a> that goes into some of the material covered in this book. 

Practically all of the examples covered in this book are done inside a VM using Vagrant. This is perfect as you don’t really need to knowhow  to setup servers or deploy applications at a production level to view how incredibly powerful Ansible is. 

Perhaps I would’ve like to see covered some advanced Ansible features like filters or more complex Jinja2 templating. On the other hand, I’m fairly new to Ansible AWX, so the Ansible Tower chapter was really helpful.

Ansible module development is not covered in this book. Quite frankly, at this point in the life of Ansible, unless you’re dealing with some sort of new service, then creating your own Ansible module is not needed (and if you think that you need to create your own module, then you’re probably doing something wrong).

It’s quite fitting that the last chapter of this book deals with Kubernetes, as the authors next book <a href="https://leanpub.com/ansible-for-kubernetes" target="_blank">Ansible for Kubernetes</a> will solely deal with using Ansible for Kubernetes app deployments. Which I would definitely purchase and read when it’s fully completed.


**Rating: 4/5**

Ansible for DevOps: Server and configuration management for humans

<a href="https://leanpub.com/ansible-for-devops" target="_blank"><img src="/assets/books/ansible-for-devops.jpg"></a>

* Chapter 1: Getting Started with Ansible
* Chapter 2:  Local Infrastructure Development: Ansible and Vagrant
* Chapter 3: Ad:Hoc Commands
* Chapter 4: Ansible Playbooks
* Chapter 5: Ansible Playbooks: Beyond the Basics
* Chapter 6: Playbook Organization: Roles, Includes, and Imports
* Chapter 7: Inventories
* Chapter 8: Ansible Cookbooks
* Chapter 9: Deployments with Ansible
* Chapter 10: Server Security and Ansible
* Chapter 11: Automating Your Automation: Ansible Tower and CI/CD
* Chapter 12: Automating HTTPS and TLS Certificates
* Chapter 13: Docker and Ansible
* Chapter 14: Kubernetes and Ansible
