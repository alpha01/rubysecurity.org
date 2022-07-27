---
categories:
  - books
  - php
tags:
  - php
  - apache
  - wordpress
layout: post
title: 'PHP: XCache performance testing'
created: 1364367122
---

Aside APC, as far as I know XCache is the second most popular PHP caching optimizer. So I manually compiled and installed XCache on my www.rubyninja.org VM and configured the WordPress W3 Total Cache plugin to use the XCache optimizer and ran the same benchmarks test that I did when APC was enabled.

After a few tests, the total requests per second was around 24-25 seconds. Slightly slower than APC. However, unlike APC, I noticed that with XCache the overall server load was less (peak at about 3.3), in addition the I/O system activity also appeared to be less than with APC. 

```bash
Concurrency Level:      5
Time taken for tests:   40.740110 seconds
Complete requests:      1000
Failed requests:        0
Write errors:           0
Non-2xx responses:      1000
Total transferred:      351000 bytes
HTML transferred:       0 bytes
Requests per second:    24.55 [#/sec] (mean)
Time per request:       203.701 [ms] (mean)
Time per request:       40.740 [ms] (mean, across all concurrent requests)
Transfer rate:          8.39 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    6 134.1      0    3000
Processing:    99  196  25.6    200     297
Waiting:       98  196  25.6    199     297
Total:         99  202 136.9    200    3209

Percentage of the requests served within a certain time (ms)
  50%    200
  66%    209
  75%    214
  80%    216
  90%    222
  95%    227
  98%    234
  99%    241
 100%   3209 (longest request)
```
