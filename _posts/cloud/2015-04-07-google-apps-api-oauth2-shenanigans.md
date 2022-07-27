---
categories:
  - cloud
  - google
tags:
  - google
  - python
layout: post
title: Google Apps API OAuth2 shenanigans
created: 1428375225
---
So I literally was just about to start flipping tables because I wasn't able to get my Google Apps API OAuth2 api verification to work. I was getting the following error:

```bash
401. That’s an error.

Error: invalid_client

no support email
```

As the documentation describes, I created my application and enabled Calendar API access to it, and lastly setup my credentials. The problem was that I was generating my OAuth 2.0 client IDs without completing the app's consent screen data. As soon as I specified my email address in my app's consent screen data and regenerated new a client ID, I was able to authenticate my application. If only the Google Developer Console would've given a warning of some sort prior to generating a client ID, a lot of #!%6@*#  moments would've been avoided.  
