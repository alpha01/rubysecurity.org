---
categories:
  - freebsd
  - pfsense
tags:
  - pf
  - freebsd
layout: post
title: Gigabit Ethernet and pfSense awesomeness
created: 1399854532
---

For quite sometime now, I've been wanting to upgrade my home network to Gigabit Ethernet. So finally the time had come to finally retired my aging Linksys WRT54GL wireless router.  Flashed with DD-WRT, my WRT54GL has served me well for well over six years. For it's replacement I opted to completely geek out with a dedicated firewall and access point solutions. For my firewall I chose pfSense. Over the last few months, I heard nothing but good things regarding this FreeBSD firewall system; primarily because of it's ease of use. This is what first attracted me to it since practically all my real firewall experience is  through administrating it through their respective web interface, ie Cisco Adaptive Security Device Manager for ASA firewalls. (Yes, I really should learn how to do this from the command line, but I digress.)

For pfsense, I used a barebore mini 1.86GHz (dual core) Atom computer. <a href="http://www.newegg.com/Product/Product.aspx?Item=N82E16856205007">OEM Production 2550L2D-MxPC Intel NM10 2 x 204Pin Intel GMA 3650 Black Mini / Booksize Barebone System</a>. For storage and memory, I had a spare of two 1GB 1066 SODIMM modules and a spare 64GB SSD drive, which is more than plenty for pfSense, if not overkill.
The install and configuration of pfSense itself is absolutely dead simple. Essentially after the install, you just need to specify which is your LAN and WAN interfaces and that's it! My WAN internet connection, is provided via DHCP and a cool thing that pfSense supports is the ability to specify a custom mac address for the new firewall machine. This is handy because it basically saved me from having to call Time Warner Cable to informed them about my new replacement networking device.
 
Although pfSense supports the addition of wireless card interfaces so it can also function as an accesses point. I opted to use a dedicated wireless access point for my wireless networking. I had Linksys E1000 wireless access that was given to me a few a months ago, so I flashed it with DD-WRT and used the Linksys E1000 as my new wireless access point. So far with this newer wireless access point and newer version of DD-WRT, I noticed that the wireless range of this new device extends much farther than then the old WRT54GL. 

The primary reason why I chose to deploy pfSense on my network besides its strong focused on security was because it's essentially a small FreeBSD base system, which has the ability to install numerous third party packages. So far I've enabled anti-virus and intrusion detection transparent proxy solutions using HAVP and Snort (this alone is fucking awesome). As well as some really cool network statistics graphing collection daemons.

With this $130.00 investment, I essentially have the equal level of capabilities that I would've otherwise have with another really fancy commercial firewall/router solution that would've cost thousands of dollars to deploy. The beauty of open source.

### To do

VLAN wired and wireless network.
