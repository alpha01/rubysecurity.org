---
categories:
  - books
  - jenkins
tags: jenkins
layout: post
hidden: true
title: The Definitive Guide to Jenkins
created: 1500100299
---

Unlike the book <a href="https://www.rubysecurity.org/Integrating-PHP-Projects-with-Jenkins" target="_blank">Integrating PHP Projects with Jenkins</a>, The Definitive Guide to Jenkins is a really good source for anyone wanting to learn Jenkins more in depth. This book describes practically almost all of the functionally that you need to know in order to start implementing rock solid Continuous Integration and Continuous Delivery solutions in whatever environment you are working in. You’ll learn how to install, configure, and secure your Jenkins itself, and more importantly how to create simple freestyle project builds to more complex pipeline project builds. The only thing <a href="https://www.rubysecurity.org/Integrating-PHP-Projects-with-Jenkins" target="_blank">Integrating PHP Projects with Jenkins</a> and this book have in common is that both books mention a lot of useful code coverage, analysis, build automation, unit/stress testing tools, etc.

One of the few drawbacks of this book (though it’s understandably given the history of the Jenkins project) is that it’s very Java focused. The working example that is companioned with this book is a working <a href="https://github.com/wakaleo/game-of-life" target="_blank">Java application</a>. At first I tried to follow along using the example Java application, but then I just skipped over the Java specific stuff and instead I applied the major concepts to my own custom Jenkins projects and builds that I have for my web applications.

Although this book was published over six years ago, most of the generic Jenkins content still applies.  The book does cover running Jenkins in your own environment as well as using and running Jenkins in a cloud environment. One thing that I was surprised was not seeing any information regarding running Jenkins in a high availability configuration; it only covered basic master/slave configurations.

Continuous Integration and Continuous Delivery is somewhat of a fairly advanced topic, thus so is this book. I think you need to have a basic to firm understanding of servers, application deployment, and a little programming knowledge to fully get the most out of this book. Thus said, we’re in July 2017 and if you’re one of those that still deploys application changes manually via FTP/SSH, then this is the book for you to start using a much better deployment strategy no matter of what application you’re working on.

### Rating: 3/5

The Definitive Guide to Jenkins: Continuous Integration for the Masses

<a href="https://www.amazon.com/Jenkins-Definitive-Continuous-Integration-Masses/dp/1449305350/" target="_blank"><img src="/assets/books/definitive-guide-to-jenkins.jpg"></a>

* Chapter 1: Introducing Jenkins
* Chapter 2: Your First Steps with Jenkins
* Chapter 3: Installing Jenkins
* Chapter 4: Configuring Your Jenkins Server
* Chapter 5: Setting Up Your Build Jobs
* Chapter 6: Automated Testing
* Chapter 7: Securing Jenkins
* Chapter 9: Code Quality
* Chapter 10: Advanced Builds
* Chapter 11: Distributed Builds
* Chapter 12: Automated Deployment and Continuous Delivery
