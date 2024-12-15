---
categories:
  - programming
  - bash
tags:
  - bash
  - rancher
  - kubernetes
layout: post
title: Simple Bash KVM Virtual Machine Startup and Shutdown Script
---

Given that I don't want my [Intel NUC](https://www.rubysecurity.org/linux/ubuntu/running-ubuntu-server-on-an-intel-nuc-10th-i7) homelab mini pc to sound like a jet engine all day. I wrote a simple bash script that would automatically startup and shutdown my kubernetes VMs easily each day. The script is cron friendly and it works like a charm.

```bash
# Start VMs
./rancher.sh start

# Stop (non-gracefully, use virsh shutdown instead)
./rancher.sh destroy 
```

```bash
#!/bin/bash
# set -x

VMS="rancher rke2"

action=$1
if [ -z $action ]; then
    echo "No action passed"
    exit 1
elif [[ "$action" != "destroy" && "$action" != "start" ]]; then
    echo "Unsupported action: $action"
    exit 1
else
    echo "Doing $action"
fi

function vm () {
    vm=$1
    current_vm_state=""
    tmp=$(virsh list --all | grep " $vm " | awk '{ print $3}')
    if ([ "x$tmp" == "x" ] || [ "x$tmp" != "xrunning" ])
    then
        echo "$vm does not exist or is shutdown!"
        current_vm_state="destroy"
    else
       echo "$vm is running!"
        current_vm_state="start"
    fi
    if [ "$action" != "$current_vm_state" ]; then
        virsh $action $vm
    fi
}

for vm in $VMS; do
    vm "$vm"
done
```
