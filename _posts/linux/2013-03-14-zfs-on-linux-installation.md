---
categories:
  - linux
  - zfs
tags:
  - zfs
  - centos
layout: post
title: 'ZFS on Linux: Installation'
created: 1363240787
---

Attending the ZFS Administration talk on SCALE 11x a couple of weeks ago made me interested in trying ZFS on Linux. Given that the speaker said that he uses ZFS on Linux on his production machines, made me think that ZFS on Linux may be finally ready for everyday use. So I'm currently looking into using the ZFS snapshots feature for my personal local file backups.

For my Linux ZFS backup server, I'm using the latest CentOS 6. Below are the steps I took to get ZFS on Linux working.

```bash
yum install automake make gcc kernel-devel kernel-headers zlib zlib-devel libuuid libuuid-devel
```

Since the ZFS modules get build using dkms, the latest dkms package will be needed. This can be downloaded from from Dell's website at <a href="http://linux.dell.com/dkms/" target="_blank">http://linux.dell.com/dkms/</a>

```bash
wget http://linux.dell.com/dkms/permalink/dkms-2.2.0.3-1.noarch.rpm
rpm -ivh dkms-2.2.0.3-1.noarch.rpm
```

Now, the `spl-modules-dkms-X` rpms need to be installed.

```bash
wget http://archive.zfsonlinux.org/downloads/zfsonlinux/spl/spl-0.6.0-rc14.src.rpm
wget http://archive.zfsonlinux.org/downloads/zfsonlinux/spl/spl-modules-0.6.0-rc14.src.rpm
wget http://archive.zfsonlinux.org/downloads/zfsonlinux/spl/spl-modules-dkms-0.6.0-rc14.noarch.rpm
rpm -ivh spl*.rpm
```

After the `spl-modules-dkms-X` rpms have been installed, the ZFS rpm packages can now be finally installed.

```bash
wget http://archive.zfsonlinux.org/downloads/zfsonlinux/zfs/zfs-0.6.0-rc14.src.rpm
wget http://archive.zfsonlinux.org/downloads/zfsonlinux/zfs/zfs-modules-0.6.0-rc14.src.rpm
wget http://archive.zfsonlinux.org/downloads/zfsonlinux/zfs/zfs-modules-dkms-0.6.0-rc14.noarch.rpm
rpm -ivh zfs*.rpm
```

One thing that confused me was that after all rpm's were installed, the `zfs` and `zfspool` binaries were no where on my system, which according to the documentation the zfs-* rpm process would have build the kernel modules and installed them on my running kernel, however this didn't look to be the case. Instead I had to do the following:

```bash
cd /usr/src/zfs-0.6.0
make
make install
```

After the install completed both zfs and zfspool utilities were available and ready to use.

Resources:

* <a href="http://zfsonlinux.org/zfs-building-srpm.html" target="_blank">http://zfsonlinux.org/zfs-building-srpm.html</a>
