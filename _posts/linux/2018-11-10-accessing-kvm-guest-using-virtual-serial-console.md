---
categories:
  - linux
  - kvm
tags: kvm
layout: post
title: Accessing KVM Guest Using Virtual Serial Console
created: 1541891038
---

For the longest time, after creating my KVM guest virtual machines, I’ve only used virt-manager afterwards to do any sort of remote non-direct ssh connection. It wasn’t until now that I finally decided to start using the <a href="https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_deployment_and_administration_guide/sect-domain_commands-connecting_the_serial_console_for_the_guest_virtual_machine" target="_blank">serial console</a> feature of KVM, and I have to say, I kind of regret procrastinating on this, because this feature is really convenient.

Enabling serial console access to a guest VM is a relatively easy process.
In CentOS, it’s simply a matter of adding the following kernel parameter to `GRUB_CMDLINE_LINUX` in `/etc/default/grub`

```bash
console=ttyS0
```

After adding the `console` kernel parameter with the value of our virtual console's device block file. Then we have to build new a grub menu and reboot:

```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```

Afterwards from the host system, you should be able to `virsh console` onto the guest VM.

The only caveat with connecting to a guest using the virtual serial console is existing the console. In my case, the way to log off the console connection was using Ctrl+5 keyboard keys. This disconnection quirk reminded me of the good old days when I actually worked on physical servers and used IPMI’s serial over network feature and it’s associated unique key combination to properly close the serial connection.

### Resources

* <a href="https://www.certdepot.net/rhel7-access-virtual-machines-console/" target="_blank">https://www.certdepot.net/rhel7-access-virtual-machines-console/</a>
* <a href="https://superuser.com/questions/637669/how-to-exit-a-virsh-console-connection" target="_blank">https://superuser.com/questions/637669/how-to-exit-a-virsh-console-connection</a>
