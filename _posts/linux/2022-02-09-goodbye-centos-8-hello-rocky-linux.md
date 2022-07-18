---
categories:
 - linux
 - centos
tags: centos
layout: post
title: "Goodbye CentOS 8, Hello Rocky Linux"
created: 1644368721
---
I'm over two months late, to the deadline as support for CentOS 8 stopped on December 31, 2021 and now the project is focusing on the CentOS 8 Stream rolling update distro. 

Instead of converting my CentOS 8 to Stream, I opted to the popular approach of just dumping CentOS in favor of Rocky Linux. The migrating process itself was super easy.  My original CentOS 8 system was NOT running the latest version of CentOS prior to its End-Of-Life.

```bash
[root@mail ~]# cat /etc/centos-release
CentOS Linux release 8.4.2105

[root@mail ~]# sudo dnf update
CentOS Linux 8 - AppStream                                                                                                                                                219  B/s |  38  B     00:00
Error: Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: No URLs in mirrorlist
```

Instead of opting to <a href="https://github.com/rocky-linux/rocky-tools/tree/main/migrate2rocky#el80-migrations" target="_blank">update the repos to point the CentOS archive vault repositories</a>. I wanted to just try the migration from my 8.4.2105 running version. After all this particular system is just a Postfix mailserver, and if it botched completely, I'm easily able to recreate the mail server using my Ansible automation.

The update process is just a matter of running the <a href="https://github.com/rocky-linux/rocky-tools/blob/main/migrate2rocky/migrate2rocky.sh" target="_blank>migrate2rocky.sh migration shell script.</a>

```bash
[root@mail migrate2rocky]# ./migrate2rocky.sh -r
migrate2rocky - Begin logging at Tue 08 Feb 2022 04:20:57 PM PST.


Removing dnf cache
Preparing to migrate CentOS Linux 8 to Rocky Linux 8.
[...]
Done, please reboot your system.
A log of this installation can be found at /var/log/migrate2rocky.log
```

After a few minutes where it applied the changes and them some package updates, the migration script ending without any errors. After rebooting the system, I was able to ssh in normally, verify that the system was in a working state, and verify Postfix still working. The migration worked!

```bash
[root@mail ~]# cat /etc/redhat-release
Rocky Linux release 8.5 (Green Obsidian)
[root@mail ~]# cat /etc/rocky-release
Rocky Linux release 8.5 (Green Obsidian)
[root@mail ~]# cat /etc/rocky-release-upstream
Derived from Red Hat Enterprise Linux 8.5
```

I even used Ansible to see what version it was reading after the migration:

```bash
[root@ansible ~]# ansible mail -m setup -a 'filter=ansible_distribution'
mail.rubyninja.org | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "Rocky",
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false
}
[root@ansible ~]# ansible mail -m setup -a 'filter=ansible_distribution_version'
mail.rubyninja.org | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution_version": "8.5",
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false
}
```

The beauty of binary compatible Linux distributions is that although updates are from new repackaged packages, when applied they work flawlessly. 

Ironically, over a decade ago I had to do similar migration from <a href="https://www.whiteboxlinux.org/" target="_blank">White Box Enterprise Linux</a> to CentOS 3.  At the time, I simply had to update the repo URLs, refresh yum, and pull down the latest updates from the CentOS 3 repos. All of which  worked beautifully.

Resources:

* https://github.com/rocky-linux/rocky-tools/tree/main/migrate2rocky
* https://www.cyberciti.biz/howto/migrate-from-centos-8-to-rocky-linux-conversion/
