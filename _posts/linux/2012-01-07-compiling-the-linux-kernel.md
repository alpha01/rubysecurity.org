---
categories:
  - linux
tags: kernel
layout: post
title: Compiling the Linux Kernel
created: 1325915587
---

A snob Linux elitist would say, "You can't call yourself a serious GNU/Linux user if you have never successfully compiled the Linux kernel at least once in your life."

The following were the steps I made to compile the Linux kernel over 4 years ago (I just happened to find my reference text file that I saved, buried within my home directory). 

1). Download kernel source code from <a href="https://www.kernel.org" target="_blank">https://www.kernel.org</a>.

2). Extract kernel source.

3). Update `EXTRAVERSION` variable on `Makefile`.

4). (Only do steps 4 if a previous kernel compilation was made within this source tree) `make mrproper` (goes through the source tree and cleans out temp files)

```bash
make mrproper
make clean
```

5). `make menuconfig` (actual configuration of the kernel compilation. Creates `.config` file)

```bash
make menuconfig
```

6). `make` (performs the actual compilation. creates bzimage file. makes the modules)

```bash
make
```

7). `make modules_install` (install modules into /lib/modules)

```bash
make modules_install
```

8). `make install` (will automatically copy the kernel and initrd file to /boot and modify the boot loader config file)

```bash
make install
```


### Reference one liner

```bash
make clean dep bzImage modules install modules_install
```
