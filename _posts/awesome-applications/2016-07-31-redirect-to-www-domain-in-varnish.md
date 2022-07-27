---
categories:
  - awesome-applications
  - varnish
tags: varnish
layout: post
title: Redirect to www domain in Varnish
created: 1469937287
---

I’m a domain name hoarder, over twenty domain names and counting.  I’ve done plenty of 301 domain name redirects in the past at the Apache level using mod_rewrite. Hell, just about every registrar now in days gives you the ability to this really easily through them.

In my case, I opted to simply point DNS directly to my server. Since I’m using Varnish (3.x), it’s more efficient to do the actual domain name redirect in Varnish itself rather than Apache. Also not to mention, the actual configuration is easier to understand in Varnish than using mod_rewrite in my opinion.

Example Varnish 3.x config:

```perl
# Tony's shitty portfolio site
sub vcl_recv {
  if (req.http.host ~ "(?i)^(www.)?(tony|antonio)baltazar\.(com|org|net)") {
       set req.http.host = "www.antoniobaltazar.com";
       error 750 "http://" + req.http.host + req.url;
   }
}

sub vcl_error {
  if (obj.status == 750) {
    set obj.http.Location = obj.response;
    set obj.status = 301;
    return(deliver);
  }
}
```

On this example, any incoming request matching `(?i)^(www.)?(tony|antonio)baltazar\.(com|org|net)` will have the host request header set to `www.antoniobaltazar.com`. The `error` function call with the status method of 750 is simply used internally by Varnish, so we can track it at the `vcl_error` subroutine. I can see why in Varnish 4.x this is handled differently since, this is basically a synthetic internal Varnish request that we're working on and should not be handled in `vcl_error`.  

Example Varnish 4 config:

```perl
sub vcl_recv {
  if (req.http.host ~ "(?i)^(www.)?(tony|antonio)baltazar\.(com|org|net)") {
    return (synth(750, ""));
   }
}

sub vcl_synth {
    if (resp.status == 750) {
        set resp.status = 301;
        set resp.http.Location = "http://www.antoniobaltazar.com" + req.url;
        return(deliver);
    }
}
```

So in Varnish 4, the configuration is even easier to understand.

An additional benefit of using this configuration is that now I’m consolidating and increasing Varnish cache hit rate. This means that Varnish won’t have two different caches for antoniobaltazar.com and www.antoniobaltazar.com.
