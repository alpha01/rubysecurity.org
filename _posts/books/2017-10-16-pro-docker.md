---
categories:
  - books
  - docker
tags: docker
layout: post
hidden: true
title: Pro Docker
created: 1508126797
---

This book is terrible, plain and simple. I bought this book as part of the <a href="https://www.humblebundle.com/books/linux-book-bundle" target="_blank">Apress Linux & Open Source Humble Bundle</a>, so I’m thankful I didn’t paid full price for it.  Before I started reading, I first thought it was going to describe and demonstrate using Docker’s advance features, and give tips and cool techniques on how to get the most of a Dockerized environment, and more importantly production Docker container deployments; I was mistaken.

This book is terrible in so many dimensions, I’m shocked Apress even decided to publish it. First, the author describes all examples using an Amazon EC2 instance as the host Docker instance. Why would the author decide to do this, when practically all of the examples could’ve worked perfectly fine in a Linux VM locally? With this in mind, he already assumed you already knew how to use Amazon Web Services, as it only provided basic information about it on the book’s appendix.

The first two chapters are the only good Docker information that you’ll find reading this book. The rest, just describes using Docker with X technology. About 90% of the content in book falls out of scope. I’m only interested in learning and using Docker, and **NOT** on using Apache Hive, for example. Instead of showcasing the usage of Docker in a bunch of environments I have no interest in, I would’ve love to see this book instead just using a single complex multi-container Docker environment that the author could’ve build along the chapters progressed in the book. Also how to use docker-compose, but that too is not mentioned! Finally, I would’ve like to see a more comprehensive and user friendly explanation of the Docker config, but Dockerfile is hardly even mentioned!

This book is terrible and would not recommend it to anyone, instead you’re better of reading the official documentation.

### Rating 1/5

Pro Docker: Learn how to use Containers as a Service for development and deployment

<a href="http://www.apress.com/us/book/9781484218297" target="_blank"><img src="/assets/books/pro-docker.png"></a>

* Chapter 1: Hello Docker
* Chapter 2: Installing Linux
* Chapter 3: Using Oracle Database
* Chapter 4: Using MySQL Database
* Chapter 5: Using MongoDB
* Chapter 6: Using Apache Cassandra
* Chapter 7: Using Couchbase Server
* Chapter 8: Using Apache Hadoop
* Chapter 9: Using Apache Hive
* Chapter 10: Using Apache Hbase
* Chapter 11: Using Apache Sqoop
* Chapter 12: Using Apache Kafka
* Chapter 13: Using Apche Solr
* Chapter 14: Using Apache Spark
