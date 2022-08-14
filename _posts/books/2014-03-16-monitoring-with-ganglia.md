---
categories:
  - books
  - ganglia
tags:
  - ganglia
  - monitoring
layout: post
hidden: true
title: Monitoring with Ganglia
created: 1394929593
---

This is essentially the handbook needed to start using Ganglia in your high performance environment. I've always been somewhat skeptical towards books that have multiple authors, however this book is definitely the exception. The authors are core contributors of Ganglia and they surely demonstrated their expert knowledge of Ganglia in this book. Hell even at times, I found myself re-reading certain portions of the book, given how complex I found their technical writing to be. Prior to reading this book I had very little experience gathering cluster metrics using Ganglia, and this book has enormously helped me understand Ganglia tremendously. The authors did and excellent job describing the different components that make up Ganglia; gmond/sflow, gmetad, and gweb.

Ganglia is a kick ass application. One thing that I really like about it that a standard install of gmond out of the box will provide the basic metrics that you would want to gather about your systems ie., memory, cpu/load, disk, and network. This book does an excellent job explaining how to extend gmond with modules written in Python. Personally I'm not big with Python, but given how easily it is to create custom modules, I can see myself using this feature if needed. Though optionally, this book also does an excellent job describing how to gather metrics using the gmetric command line utility, of which at this point you're not tied to any specific language if you want to gather custom metrics. Additionally this book included some examples using libaries that make use of gmetric functionally to gather metrics about your application. The primary reason why I bought this book was because I wanted to learn more in depth about Ganglia integration with Nagios, and this book does an excelent job describing how easily Ganglia and Nagios alert integration can be enabled making an absolute kick ass monitoring combo!

One unique thing that this book included was a chapter dedicated to real case studies of Ganglia being used in large high performance environments, and the unique constraints that each environment had, what they did to solve or mitigate the problem. The only thing that perhaps this book could have excluded was the portion regarding extending gmond using C/C++, given that the core audience for this book are system engineers, and given the complexity of the subject, I feel extending gmond using C/C++ falls out of scope of the book.

As far as I know this is the only Ganglia book available, and probably the only book you'll need to learn Ganglia. I definitely recommend this book if you're interested deploying Ganglia.

### Rating: 5/5

Monitoring with Ganglia: Tracking Dynamic Host Application Metrics at Scale

<a href="http://www.amazon.com/Monitoring-Ganglia-Matt-Massie/dp/1449329705" target="_blank"><img src="/assets/books/monitoring-with-ganglia.jpg"></a>

* Chapter 1: Introducing Ganglia
* Chapter 2: Installing and Configuring Ganglia
* Chapter 3: Scalability
* Chapter 4: The Ganglia Web Interface
* Chapter 5: Managing and Extending Metrics
* Chapter 6: Troubleshooting Ganglia
* Chapter 7: Ganglia and Nagios
* Chapter 8: Ganglia and sFlow
* Chapter 9: Ganglia Case Studies
