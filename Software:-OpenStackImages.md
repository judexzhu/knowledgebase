# OpenStack ready images

## Overview

 * Guide to building various OpenStack ready images

## Requirements
 * Any RHEL/Fedora build with cloud-utils cloud-init cloud-utils-growpart installed 

## CentOS 
### 6.6 

Run the following to start the install, pulling from public mirrors 

```
virt-install --connect=qemu:///system --network=bridge:virbr0 \
--extra-args="console=tty0 console=ttyS0,115200 serial rd_NO_PLYMOUTH" \
--name=centos6.6 \
--disk path=/var/lib/libvirt/images/centos6.6.qcow2,format=qcow2,size=10,cache=none \
--ram 8192 --vcpus 6 --check-cpu --accelerate --os-type linux \
--location=http://mirror.i3d.net/pub/centos/6/os/x86_64/ --nographics
```

### 7.1 

## Ubuntu
### 14.04/trusty