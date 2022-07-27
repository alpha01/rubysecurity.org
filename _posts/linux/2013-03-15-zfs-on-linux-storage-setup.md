---
categories:
  - linux
  - zfs
tags:
  - ubuntu
  - centos
  - virtualbox
  - zfs
layout: post
title: 'ZFS on Linux: Storage setup'
created: 1363312817
---

For my media storage, I'm using a 500GB 5400 RPM USB drive. Since my Linux ZFS backup server is a virtual machine under VirtualBox, in order for the VM to be able to access the entire USB drive completely, the VirtualBox  Extension Pack add-on needs to be installed.

The VirtualBox  Extension Pack for all versions can be found on the <a href="http://download.virtualbox.org/virtualbox/" target="_blank">VirtualBox website</a>. **It is important that the Extension Pack installed must be for the same version as VirtualBox~**

<img src="/assets/linux/virtualbox_about.png" alt="VirtualBox about" title="VirtualBox about" />

```bash
wget http://download.virtualbox.org/virtualbox/4.1.12/Oracle_VM_VirtualBox_Extension_Pack-4.1.12.vbox-extpack
VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-4.1.12.vbox-extpack
```

Additionally, it is also important that the user which VirtualBox will run under is member of the `vboxusers` group.

```bash
groups tony
tony : tony adm cdrom sudo dip plugdev lpadmin sambashare
sudo usermod -G  adm,cdrom,sudo,dip,plugdev,lpadmin,sambashare,vboxusers tony
groups tony
tony : tony adm cdrom sudo dip plugdev lpadmin sambashare vboxusers
```

Since my computer is already using two other 500GB external USB drives, I had to properly identify the drive that I wanted to use for my ZFS data. This was a really simple process (I don't give a flying fuck about sharing my drive's serial).

```bash
sudo hdparm -I /dev/sdd|grep Serial
	Serial Number:      J2260051H80D8C
	Transport:          Serial, ATA8-AST, SATA 1.0a, SATA II Extensions, SATA Rev 2.5, SATA Rev 2.6; Revision: ATA8-AST T13 Project D1697 Revision 0b
```

Now that I know the serial number of the USB drive, I can configure my VirtualBox Linux ZFS server VM to automatically use the drive.

<img src="/assets/linux/virtualbox_drive_info.png" alt="VirtualBox drive configuration" title="VirtualBox drive configuration" />

At this point I'm about to use the 500 GB hard drive as /dev/sdb under my Linux ZFS server and use it to create ZFS pools and file systems.

```bash
zpool create pool backups /dev/sdb
zfs create backups/dhcp
```

Since I haven't used ZFS on Linux extensively before, I'm manually mounting my ZFS pool after a reboot.

```bash
root@backup ~]# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/VolGroup-lv_root
                      3.5G  1.6G  1.8G  47% /
tmpfs                 1.5G     0  1.5G   0% /dev/shm
/dev/sda1             485M   67M  393M  15% /boot
[root@backup ~]# zpool import
   pool: backups
     id: 15563678275580781179
  state: ONLINE
 action: The pool can be imported using its name or numeric identifier.
 config:

	backups     ONLINE
	  sdb       ONLINE
[root@backup ~]# zpool import backups
[root@backup ~]# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/VolGroup-lv_root
                      3.5G  1.6G  1.8G  47% /
tmpfs                 1.5G     0  1.5G   0% /dev/shm
/dev/sda1             485M   67M  393M  15% /boot
backups               446G  128K  446G   1% /backups
backups/afs           447G  975M  446G   1% /backups/afs
backups/afs2          447G  750M  446G   1% /backups/afs2
backups/bashninja     448G  1.4G  446G   1% /backups/bashninja
backups/debian        449G  2.5G  446G   1% /backups/debian
backups/dhcp          451G  4.4G  446G   1% /backups/dhcp
backups/macbookair    446G  128K  446G   1% /backups/macbookair
backups/monitor       447G  880M  446G   1% /backups/monitor
backups/monitor2      446G  128K  446G   1% /backups/monitor2
backups/rubyninja.net
                      446G  128K  446G   1% /backups/rubyninja.net
backups/rubysecurity  447G  372M  446G   1% /backups/rubysecurity
backups/solaris       446G  128K  446G   1% /backups/solaris
backups/ubuntu        446G  128K  446G   1% /backups/ubuntu
```
