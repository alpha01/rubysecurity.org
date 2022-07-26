---
categories:
  - awesome-applications
  - prometheus
tags:
 - prometheus
 - nagios
layout: post
title: RIP Nagios
created: 1623032971
---
It's an end of an era, at least with me using <a href="https://www.nagios.org/" target="_blank">Nagios</a> or Nagios Core to be exact.  Unless you've been living under a rock, <a href="https://prometheus.io/" target="_blank">Prometheus</a> has become the defacto tool when it comes to system monitoring.  While professionally, I stoped using Nagios a few years ago, but I still kept a Nagios server running in my HomeLab for internal monitoring along side Prometheus. What kept me from fully dumping Nagios, was having to migrate some of my custom alerts.  However, this weekend I finally decided to give Nagios its final blow and migrate my custom alerts to Prometheus. With the help of the awesome <a href="https://github.com/prometheus/blackbox_exporter" target="_blank">Blackbox exporter</a>, I was able to easily port over my custom http and dns alerts to Prometheus.

Like Nagios, I feel Prometheus also has a steep learning curve.  However, overall I feel the benefits Prometheus brings like integration with cloud native system infrastructures, definitely outweigh the drawbacks of this awesome monitoring tool.

![Prometheus](/assets/awesome-applications/prometheus.png)
