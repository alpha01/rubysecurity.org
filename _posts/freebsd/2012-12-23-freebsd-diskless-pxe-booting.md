---
categories:
  - freebsd
  - pxe
tags: freebsd
layout: post
title: FreeBSD diskless PXE booting
created: 1356304716
---

After a couple of trial and error tests and lots of caffeine ingested, I finally managed to install FreeBSD 9.1 over my network completely diskless using ISC's DHCP, PXE, tftpd-hpa, and NFS.

1). Download iso image and copy over all files. (Where the `/srv/tftp/freebsd/amd64` directory is the root directory of the iso files)

```bash
wget ftp://ftp.freebsd.org/pub/FreeBSD/releases/amd64/amd64/ISO-IMAGES/9.0/FreeBSD-9.0-RELEASE-amd64-disc1.iso
mount -o loop FreeBSD-9.0-RELEASE-amd64-disc1.iso /mnt
mkdir -p /srv/tftp/freebsd/amd64
cp -a /mnt/* /srv/tftp/freebsd/amd64
cp -a /mnt/.cshrc /srv/tftp/freebsd/amd64
cp -a /mnt/.profile /srv/tftp/freebsd/amd64
cp -a /mnt/.rr_moved /srv/tftp/freebsd/amd64
```

2). Create the following additional directories:

```bash
mkdir /srv/tftp/freebsd/amd64/jails
mkdir -p /srv/tftp/freebsd/amd64/conf/base/jails
mkdir /srv/tftp/freebsd/amd64/conf/default
chmod -R 777 /srv/tftp/freebsd/amd64/conf
chmod -R 777 /srv/tftp/freebsd/amd64/jails
```

3). Edit `/srv/tftp/freebsd/amd64/etc/fstab`, comment out the entry in the file:

```bash
#/dev/iso9660/FREEBSD_INSTALL / cd9660 ro 0 0
```

4). Add the following entry to `/srv/tftp/freebsd/amd64/etc/rc.conf`:

```bash
root_rw_mount="NO"
```

5). NFS configuration:

```bash
/srv/tftp/freebsd/amd64		192.168.1.1/24(ro,sync,no_root_squash,no_subtree_check)
```

6). dhcpd configuration (of course, IP may differ depending on your environment):
* `192.168.1.128` will be the IP that wil be assigned to the new FreeBSD system.
* `192.168.1.2` is the IP of the TFTP/NFS server where the installation files are stored in.

The filename path is relative to what path you configured with tftpd-hpa.

```bash
host freebsdboot {
  hardware ethernet 08:00:27:2b:f9:f8;
  fixed-address 192.168.1.128;
  filename "freebsd/amd64/boot/pxeboot";
  option root-path "192.168.1.2:/srv/tftp/freebsd/amd64";
}
```

<img src="/assets/freebsd/FreeBSD_diskless_PXE.png" alt="FreeBSD Diskless PXE" title="FreeBSD Diskless PXE image" />


References:

* <a href="http://forums.freebsd.org/showthread.php?t=30069" target="_blank">http://forums.freebsd.org/showthread.php?t=30069</a>
* <a href="http://lists.freebsd.org/pipermail/freebsd-questions/2012-March/238969.html" target="_blank">http://lists.freebsd.org/pipermail/freebsd-questions/2012-March/238969.html</a>
* <a href="http://box.matto.nl/disklessfreebsd.html" target="_blank">http://box.matto.nl/disklessfreebsd.html</a>
