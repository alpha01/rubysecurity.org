---
categories:
  - awesome-applications
  - varnish
tags:
  - varnish
  - apache
  - wordpress
  - testing
layout: post
title: Varnish WordPress Performance Testing
created: 1385676825
---

Thanks to my new job, I've been working a lot with Varnish. Man, Varnish is one kick ass HTTP web accelerator! A few months back I ran a few performance Apache performance tests on my WordPress site with different layers of caching enabled:

* <a href="https://www.rubysecurity.org/php_xcache" target="_blank">https://www.rubysecurity.org/php_xcache</a>
* <a href="https://www.rubysecurity.org/apache_stress-testing" target="_blank">https://www.rubysecurity.org/apache_stress-testing</a>

So now I wanted to see how the results may differ using Varnish.

### Configuration

At a bare minimal, Varnish needs to be configured to remove cookies set by WordPress in order to make the content cacheable.

```perl
sub vcl_recv {
  # Drop any cookies sent to Wordpress.
  if (!(req.url ~ "wp-(login|admin)") && req.http.host ~ "rubyninja.org") {
    unset req.http.cookie;
  }
}

sub vcl_fetch {
  # Drop any cookies Wordpress tries to send back to the client.
  if (!(req.url ~ "wp-(login|admin)") && req.http.host ~ "rubyninja.org") {
    unset beresp.http.set-cookie;
  }
}
```

With this configuration enabled, I ran the same identical ab tests that used to benchmark my server previously.

```bash
[root@rubyninja ~]# ab -n 1000 -c 5 http://www.rubyninja.org/index.php
This is ApacheBench, Version 2.0.40-dev <$Revision: 1.146 $> apache-2.0
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Copyright 2006 The Apache Software Foundation, http://www.apache.org/

Benchmarking www.rubyninja.org (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Finished 1000 requests


Server Software:        Apache/2.2.15
Server Hostname:        www.rubyninja.org
Server Port:            80

Document Path:          /index.php
Document Length:        0 bytes

Concurrency Level:      5
Time taken for tests:   39.528015 seconds
Complete requests:      1000
Failed requests:        0
Write errors:           0
Non-2xx responses:      1001
Total transferred:      374139 bytes
HTML transferred:       0 bytes
Requests per second:    25.30 [#/sec] (mean)
Time per request:       197.640 [ms] (mean)
Time per request:       39.528 [ms] (mean, across all concurrent requests)
Transfer rate:          9.23 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:       87  100 134.1     93    3092
Processing:    88   95  10.7     91     220
Waiting:       87   94  10.6     90     218
Total:        177  196 134.4    186    3183

Percentage of the requests served within a certain time (ms)
  50%    186
  66%    192
  75%    196
  80%    199
  90%    207
  95%    217
  98%    234
  99%    251
 100%   3183 (longest request)
```

The requests per second handled by the webserver weren't much different from the caching layer that I already had enabled previously, which is the WordPress W3 Total Cache plugin configured with Page, Database, Object, and Browser cache enabled using APC, and mod_pagespeed.

However the huge difference is that all requests **_never_** reached the Apache backend. Varnish cached all content and served all the requests directly.  Varnish is just fucking awesome.
