---
categories:
  - books
  - monitoring
tags: monitoring
layout: post
hidden: true
title: The Art of Monitoring
created: 1505798488
---

The Art of Monitoring is a modern and fairly complex guide of monitoring your application effectively. There is no doubt, nowadays just about any modern application has a complex architecture, with many moving parts surrounding it. This book aims to give you the best guide to monitor your entire application stack. From the operating system, web server, to the application itself. The example applications used in this book are Ruby on Rails and Java based, but the book is written in a manner that I was able to apply some of the concepts on my existing PHP applications. The examples are described using both Ubuntu and CentOS systems, and I loved that the author provided links to all three major configuration management tools Chef, Puppet, and Ansible; with their respective configuration to deploy whatever service was being described as we're reading this book. 

The bread and butter of this book is the use of the event-processing and tracking based application called <a href="http://riemann.io/" target="_blank">Riemann</a>. The core monitoring concepts are handled by this application. I have to admit, prior to reading this book, I’ve never heard of this application and I had a hard time following the examples showcasing its use. While I did had some minor experience using some commercial application event-processing based solutions (New Relic), grasping the notion of having <a href="http://riemann.io/" target="_blank">Riemann</a> be the central point of the monitoring framework was hard to me accept. Mainly because some of the functionally described by <a href="http://riemann.io/" target="_blank">Riemann</a> was of alerting after certain thresholds were met. To me, this is something that I’ve been doing in Nagios for years. While I can see why the integration of <a href="http://riemann.io/" target="_blank">Riemann</a> is showcased here, as it's deeply tied to the flow of other monitoring tools that are also described in this book. That's the addition of the ELK stack for log analysis, collectd/statds for system and application metrics, Graphite for storing the metrics, and Grafana for displaying the application metrics.

 This book covers just about every aspect of a modern application that you want to monitor, with the exception of the networking layer. From the ground up, the core audience of this book are system administrators and developers. I liked that this book designed the entire monitoring stack to easily scale depending on your application environment. As the author describe it, "your monitoring framework should be just as dynamic as your application."

**Rating: 3/5**

The Art of Monitoring

<a href="https://www.artofmonitoring.com/" target="_blank"><img src="/assets/books/the-art-of-monitoring.jpg"></a>

* Chapter 1: An Introduction to Monitoring
* Chapter 2: Monitoring, Metrics and Measurement
* Chapter 3: Events and metrics with Riemann
* Chapter 4: Storing and graphing metrics, including Graphite and Grafana
* Chapter 5: Host-based monitoring with collectd
* Chapter 6: Monitoring hosts and services
* Chapter 7: Containers - another kind of host
* Chapter 8: Logs and Logging, covering structure logging and the ELK stack
* Chapter 9: Building monitored applications
* Chapter 10: Alerting and Alert Management
* Chapters 11-13: Monitoring an application and stack
