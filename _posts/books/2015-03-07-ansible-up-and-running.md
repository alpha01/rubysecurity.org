---
categories:
  - books
  - ansible
tags: ansible
layout: post
hidden: true
title: Ansible Up & Running
created: 1425701997
---

Like the book <a href="https://www.rubysecurity.org/books_ansible-configuration-management" target="_blank">Ansible Configuration Management</a> I reviewed a few days ago, Ansible Up & Running is a short starter book. As the time of this review, the copy of the book I read is still considered to be an early release raw unedited version. This short book, attempts to cover all the basic material needed to effectively use Ansible as quick as possible. Or as I like to call it, "don't tell me your life story, I just want to use your fucking shit".

I personally felt this book was slightly aimed for developers. Unlike the book <a href="https://www.rubysecurity.org/books_ansible-configuration-management" target="_blank">Ansible Configuration Management</a> where the examples expected you to use Linux machines, in this book the examples were described using Vagrant instead. Another difference between them, is that this book does a better job describing YAML and its syntax in a much more understandable manner. Which I think is critical given that YAML plays a critical role in Ansible. Additionally this book does an excellent job covering the Jinja2 template system that Ansible uses.

As for the cons, this book hardly mentions the use of roles in Ansible. Additionally it doesn't cover anything regarding creating custom modules. Only development topic covered was the creation of a custom dynamic inventory script using Python. Which I don't why the author decided to this, since in my opinion developing custom modules, lookup, var, and filter plugins is a much more important and useful topic for developers and system administrators to have a knowledge of. Also considering that Ansible doesn't have any official documentation regarding developing plugins, this alone would've been an excellent selling point for this book.

Unlike <a href="https://www.packtpub.com/networking-and-servers/ansible-configuration-management" target="_blank">Ansible Configuration Management</a> which PacktPub is charging an arm and a leg for, this e-book can be freely downloaded right now. So get it while it last! 

### Rating: 3/5

Ansible: Up & Running. Automating Configuration Management and Deployment the Easy Way

<a href="http://www.ansible.com/blog/free-ansible-book" target="_blank"><img src="/assets/books/ansible_up_and_running.jpg"></a>
