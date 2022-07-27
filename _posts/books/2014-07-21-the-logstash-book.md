---
categories:
  - books
  - logstash
tags: logstash
layout: post
hidden: true
title: The Logstash Book
created: 1405904716
---

Logstash, along with the entire ELK stack, is a really awesome piece of software. Assuming you already have moderate *nix knowledge (after all this book is aimed for SysAdmins/Operations, Developers and DevOps folks), this short eBook is practically all you need to start learning and using Logstash. A good chunck of this book are excepts from the official documentation. Like most FOSS projects, the official documentation is somewhat null, and this book does a really good job filling that void in a very comprehensive manner.

This book does an excellent job walking through setting up the entire Logstash, Elasticsearch, and Kibana stack, and the basic minimal configurations needed to get up and running quickly, as well as an entire chapter devoted to scaling your Logstash/Elasticsearch infrastructure. The core content of the eBook, is regarding the three main configuration options in  Logstash; input, filter and output. Which is how Logstash is able to  receive logs via different ways like syslog, another Logstash deamon, and logstash-forwarder (aka lumberjack), and then how we filter the incoming logs to a more well comprehensive structure using various filter plugins, and finally configuring Logstash how to save our incoming filtered logs to a respective backend.  
This book also describes how we can integrate Logstash with other applications like Graphite and Nagios. As well a high-level introduction on how to extend Logstash by creating custom input and filter plugins.

Along with Logstash itself, this book is very Linux centric. Practically nothing is mentioned regarding Logstash in other platforms like FreeBSD and Solaris aside from configuring them to ship their logs to a central Logstash server via syslog.

Thus said at just $10, this eBook is worth every penny of it.

### Rating: 5/5

The Logstash Book

<a href="https://www.logstashbook.com" target="_blank"><img src="/assets/books/logstash.png"></a>

* Chapter 1: Introduction or Why Should I Bother?
* Chapter 2: Getting Started with Logstash
* Chapter 3: Shipping Events
* Chapter 4: Shipping Events without the Logstash agent
* Chapter 5: Filtering Events with Logstash
* Chapter 6: Outputting Events from Logstash
* Chapter 7: Scaling Logstash
* Chapter 8: Extending Logstash
