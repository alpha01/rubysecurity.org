---
categories:
  - programming
  - ruby
tags: ruby
layout: post
title: 301 URL redirect on Ruby on Rails
created: 1336204036
---

In your respective controller, specify the method of the view in questioned that needs to be redirected.

```ruby
def code
   headers["Status"] = "301 Moved Permanently"
   redirect_to "https://www.example.com/alpha01"
 end
```
