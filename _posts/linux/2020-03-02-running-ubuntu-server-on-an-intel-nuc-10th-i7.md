---
categories:
  - linux
  - ubuntu
tags: ubuntu
layout: post
title: Running Ubuntu Server on an Intel NUC 10th i7
created: 1583111666
---

Late last year, I purchased a secondary <a href="https://www.intel.com/content/www/us/en/products/boards-kits/nuc/kits/nuc8i3beh.html" target="_blank">Intel NUC 8th i3</a> for my homelab. My main goal was to use this secondary NUC primarily to learn Mesos and Kubernetes more in depth.  Little that I knew that the dual core i3 on the NUC was not truly powerful enough to run a simple ten node DC/OS cluster, let alone another Kubernetes cluster on the same machine.  So I decided to wait until the new <a href="https://www.intel.com/content/www/us/en/products/boards-kits/nuc/kits/nuc10i7fnhc.html" target="_blank">i7 10th generation Intel NUCs</a> were released, so I can upgrade.

The upgrade itself was not as easy as I first imagine. Both the RAM and hard drive were swapped from the old 8th gen NUC to the new 10th gen NUC. Ubuntu started up successfully, and all the memory was properly recognized on the new machine, however networking was not working. My first thought was that since now Linux was running on a new hardware, I needed to remove the old NIC's udev configuration. I soon realized that apparently in the post systemd world, we no longer need to do this. After a quick Google search, I found a Reddit post that outline my exact problem. <a href="https://www.reddit.com/r/intelnuc/comments/eox6k1/caution_new_frost_canyon_nucs_have_an_integrated/" target="_blank">https://www.reddit.com/r/intelnuc/comments/eox6k1/caution_new_frost_canyon_nucs_have_an_integrated/</a>

I was shocked to learn that the new 10th gen NUC’s network card is so new that it doesn’t even have its driver on the latest Ubuntu Server LTS! Luckily compiling and loading the newer e1000e driver was a really easy task. The only caveat was that I had to go into the UEFI Bios and disable secure boot and allow 3rd party modules, otherwise the new kernel module would fail to load.

After a few hours of usage, performance is completely night a day. The new 10th gen i7 hex core processor completely blows 8th gen i3 dual core, out of the water.
