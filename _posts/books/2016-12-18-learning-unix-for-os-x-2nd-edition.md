---
categories:
  - books
  - macos
tags: macos
layout: post
hidden: true
title: Learning Unix for OS X, 2nd Edition
created: 1482031403
---

Learning Unix for OS X is an easy to learn introduction to the Unix command line. Since Bash is the default shell Apple ships OS X with, all examples in this book are done using the Bash shell. This book will basically hold your hand through the entire process of learning how to work in the command line environment. You’ll learn simple things like creating files and directories, working with symlinks and hardlinks, shell aliases, shell I/O, Unix pipes, and a generic overview of the Unix file system directory structure. Perhaps one of the most important things someone can get out of this book is learning how to use Vi/Vim. This book will teach you the basics of Vim, ie; editing new or existing files, searching/replace text, etc.
 
I was pleasantly surprised that this book covered basic network command line utilities like `ssh` and `curl`. If anything, it would've been helpful if it also mentioned `rsync`, given how incredibly useful that program is. 

One important aspect before start reading this book is that it assumes the reader doesn’t know anything about Unix culture. So anyone who reads this book will get up to date with the generic Unix terminologies (and in the process becoming a legit computer geek)

By the end of this book, the reader will have a basic understanding of all the following core Unix utilities to name a few:

```bash
cd
ls
mkdir
ln
grep
find
xargs
chown 
chmod
du
sort
uniq
tr
wc
ssh
ftp
curl
```

Although it may technically fall out of scope, it would’ve been great if this book covered  <a href="http://brew.sh/" target="_blank">homebrew</a>.  The reason being is that practically any sysadmin or developer using MacOS relies on <a href="http://brew.sh/" target="_blank">homebrew</a> in way or another. It's become a really important tool in the Mac OS X ecosystem, that essentially follows other Unix like operating systems of how software is traditionally distributed, installed, and updated. 

Regular expressions are briefly covered in book. In my opinion the regex material covered is enough for basically 90% of your text matting pattern needs. Anyone totally new to regular expressions can confidently have the basic knowledge of pattern matching after reading this book. 

This book was published on January 2016, and I feel the chapter on X11 is practically useless nowadays. Aside from games, I can't recall a time I needed X11 on my OS X system. It took years for all major X11 applications that I used over the last decade, for example OpenOffice, GIMP, Wireshark, to name a few. To all be ported and run natively on OS X without the need of having X11 installed.

Overall, this is book is aimed for Mac users that want to be more comfortable using the command line. In my opinion this mostly means developers and designers that for some reason are not familiar with Unix and want take full advantage of the amazing power of Unix. This book does not cover shell programming. So the next logical step for anyone that read this book is to start learning shell programming to take their Unix knowledge to another level.


**Rating: 3/5**

Learning Unix for OS X, 2nd Edition:  Going Deep With the Terminal and Shell

<a href="http://shop.oreilly.com/product/0636920045595.do" target="_blank"><img src="/assets/books/learning-unix-for-osx.png"></a>

* Chapter 1: Why Use Unix?
* Chapter 2: Using the Terminal
* Chapter 3: Exploring the Filesystem
* Chapter 4: File Management
* Chapter 5: Finding Files and Information
* Chapter 6: Redirecting I/O
* Chapter 7: Multitasking
* Chapter 8: Taking Unix Online
* Chapter 9: Of Windows and X11
* Chapter 10: Where to Go from Here
