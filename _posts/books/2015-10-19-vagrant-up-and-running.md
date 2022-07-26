---
categories:
  - books
  - vagrant
tags: vagrant
layout: post
hidden: true
title: 'Vagrant: Up and Running'
created: 1445230499
---

Vagrant is an awesome piece of software. This fantastic application has changed the way most companies do development. Vagrant: Up and Running, is an introductory book that extends the official Vagrant document in a short well structure approach. Practically all of the topics covered in the book are covered in the official Vagrant documentation. However, this book is aimed for people completely new Vagrant and are unaware of the idiosyncrasy of what it takes to configure a server. The book's examples are using VirtualBox as the visualization platform, and Linux as the VM guest. So in addition of learning Vagrant, you'll also be learning some of the really cool in features of VirtualBox and see how wonderfully Vagrant abstracts this by using it's Vagrantfile guest configuration.

The book is divided into seven chapters. The first two chapters are basic introductions to Vagrant. Their is also a chapter dedicated to provisioning a guest using Chef and Puppet. The provisioning chapter does not go into the details of Chef or Puppet, but the example Chef recipes and Puppet manifest described on the book are really simple and straight forward. They are well described, and should be able to easily understand the Vagrant provisioning process no matter what provision tool you'll later end up using. My favorite chapter, and practically the reason anyone needs to buy this book, is the chapter on Plug-in development. I've been hacking on a Vagrant plugin these last few days, and personally feel the official documentation is lacking in this area, and this chapter is hands down the best documentation on Plug-in development.  


The only con of this book, is not necessarily the book's fault but rather a result of Vagrant's constant fast pace development. The book was published on May 2013, and being at this point in time over two years old, it's diffidently aging fast. Some of the examples on the Extending Vagrant with Plug-ins chapter, using the lasted version of Vagrant at the time of this writing, spit out deprecate warnings.  

<iframe width="560" height="315" src="https://www.youtube.com/embed/WfmRFNpgEp8?rel=0" frameborder="0" allowfullscreen></iframe>

### Rating: 4/5

Vagrant: Up and Running. Create and Manage Virtualized Development Environments

<a href="http://shop.oreilly.com/product/0636920026358.do" target="_blank"><img src="/assets/books/vagrant-up-and-running.jpg"></a>

* Chapter 1: An Introduction to Vagrant
* Chapter 2: Your First Vagrant Machine
* Chapter 3: Provisioning Your Vagrant VM
* Chapter 4: Networking in Vagrant
* Chapter 5: Modeling Multimachine Clusters
* Chapter 6: Boxes
* Chapter 7: Extending Vagrant with Plug-Ins
