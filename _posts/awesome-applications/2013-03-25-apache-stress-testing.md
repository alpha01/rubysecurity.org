---
categories:
  - awesome-applications
  - apache
tags:
  - apache
  - php
  - wordpress
layout: post
title: Apache Stress Testing
created: 1364182755
---

As I didn't have anything much better to do a Sunday afternoon, I wanted to get some benchmarks on my Apache VM that's hosting my blog www.rubyninja.org. I've used the <a href="http://httpd.apache.org/docs/2.2/programs/ab.html">ab</a> Apache benchmarking utility in the past to simulate high load on a server but have not used it on benchmarking Apache in detail.

My VM has a single shared Core i5-2415M 2.30GHz CPU with 1.5 GB of RAM allocated to it.

I based made my benchmarks using a total of 1000 requests with 5 concurrent requests at a time.

```bash
ab -n 1000 -c 5 http://www.rubyninja.org/index.php
```

### Results:

Using just the `mod_pagespeed` Apache module enabled.

```bash
Time taken for tests:   154.687976 seconds
Complete requests:      1000
Failed requests:        0
Write errors:           0
Non-2xx responses:      1000
Total transferred:      351000 bytes
HTML transferred:       0 bytes
Requests per second:    6.46 [#/sec] (mean)
Time per request:       773.440 [ms] (mean)
Time per request:       154.688 [ms] (mean, across all concurrent requests)
Transfer rate:          2.21 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.1      0       3
Processing:   328  772  46.4    772    1040
Waiting:      327  771  46.4    772    1040
Total:        328  772  46.4    772    1040
```


Using `mod_pagespeed` and `APC` enabled.

```bash
Time taken for tests:   41.355400 seconds
Complete requests:      1000
Failed requests:        0
Write errors:           0
Non-2xx responses:      1000
Total transferred:      351000 bytes
HTML transferred:       0 bytes
Requests per second:    24.18 [#/sec] (mean)
Time per request:       206.777 [ms] (mean)
Time per request:       41.355 [ms] (mean, across all concurrent requests)
Transfer rate:          8.27 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    6 134.1      0    3000
Processing:    88  199  28.4    202     459
Waiting:       88  199  28.4    201     459
Total:         88  205 137.2    202    3208
```


Using the WordPress W3 Total Cache plugin configured with Page, Database, Object, and Browser cache enabled the APC caching method and `mod_pagespeed`.

```bash
time taken for tests:   37.750269 seconds
Complete requests:      1000
Failed requests:        0
Write errors:           0
Non-2xx responses:      1000
Total transferred:      351000 bytes
HTML transferred:       0 bytes
Requests per second:    26.49 [#/sec] (mean)
Time per request:       188.751 [ms] (mean)
Time per request:       37.750 [ms] (mean, across all concurrent requests)
Transfer rate:          9.06 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    5 133.9      0    2996
Processing:    74  181  26.6    185     315
Waiting:       74  181  26.6    184     314
Total:         74  187 136.4    185    3178
```

As you can see, APC is the once caching method that makes a huge difference. Without APC, the server response time was just 6.46 requests per second and the load average peaked at about 12, while with the default APC configuration enabled, the server response time was 24.18 requests per second, with a load average peaking about 3. Adding the W3 Total Cache WordPress plugin helped performance slightly more, from 24.18 requests per second to 26.49 requests per second (load was about the same, including I/O activity). One interesting thing that I noticed is that with caching enabled, that is APC, the I/O usage spiked considerably. Most notably, MySQL was the high cpu usage process when doing the benchmarks. Since the caching is based in memory at this point it appears that the bottleneck in the web application is MySQL.
