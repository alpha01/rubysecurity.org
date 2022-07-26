---
categories:
  - awesome-applications
  - docker
tags:
  - docker
  - security
layout: post
title: Log into a Docker Container as root
created: 1540711654
---

```bash
docker exec -u 0 -it mycontainer bash
```
