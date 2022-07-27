---
categories:
  - solaris
  - useradd
tags: solaris
layout: post
title: User creations in Solaris 11
created: 1365824789
---
To my surprise Solaris 11 does not create new user's home directory by default. 

### Errors:

```bash
root@solaris:~# su - testuser
su: No directory!
root@solaris:~# pwck
[....]
testuser:x:106:10::/export/home/testuser:/usr/bin/bash
        Login directory not found
```

### Fix:

```bash
root@solaris:~# useradd -m testuser
80 blocks
```

In the process, I learned something new about the su command. In Linux, when switching from root to a limited user, I used to do the following:

```bash
[root@rubyninja ~]# su tony -
```

What I did not know was that the above command will indeed load up the PATH of tony, but it will also append root's PATH at end of it which is kind of scary. In theory the command that I wanted to use was `su - username`,  luckily this feature is not supported in Solaris 11.

```bash
root@solaris:/# su testuser -
bash: /root/.bashrc: Permission denied
```
