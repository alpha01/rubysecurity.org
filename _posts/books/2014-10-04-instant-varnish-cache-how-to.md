---
categories:
  - books
  - varnish
tags: varnish
layout: post
hidden: true
title: Instant Varnish Cache How-to
created: 1412381960
---

As a prerequisite, anyone wanting to use Varnish in order to effectively scale their web applications, it's absolutely critical that they know the HTTP protocol in depth, in particular HTTP Cache headers.  This book is short, but extremely technical. Anyone wanting to use Varnish and they are just using this book,  would have an extremely hard time reading this book and implementing Varnish in their environment, if they are not familiar how the HTTP protocol functions. I wasn't quite sure why this book covered HTTP caching headers almost the end since it's such a vital for anyone to know before hand. It would've been much better if that was described in the first few chapters. Thus said, already having the HTTP protocol knowledge, this book is a really good resource for anyone already familiar  with Varnish but have some questions in areas they aren't quite sure they are implemented or handled in by Varnish. The  book covers setting up Varnish as a standalone HTTP  web accelerator as well as load balancer. It does a good job describing how handles HTTP requests, and how they are processed through it's different subroutines, and more importantly how we can alter their behavior, ie drop cookies, force caching, restart request to a differ backend, etc. using the Vanish Configuration Language. The VCL  examples described on this book are extremely simple, personally I would've liked if the examples were more production scenario like. 

One major thing that this book lacked off was the in depth overview of the available built in variable objects in Varnish; `req`, `bereq`, `beresp`, `obj`, `resp`, `now`, `client`, and `server`. Knowing this information is important if you fully want to effectively deploy Varnish.

If this book is used along with the official Varnish documentation, then I would definitely suggest it as secondary resource, but for anyone new to Vanish, I would suggest to read the documentation first prior to reading this book.

### Rating: 3/5

Instant Varnish Cache How-to: Hands-on recipes to improve your websites's load speed and overall user experience using Varnish Cache

<a href="https://www.packtpub.com/hardware-and-creative/instant-varnish-cache-how-instant" target="_blank"><img src="/assets/books/instant-varnish-how-to.jpg"></a>
