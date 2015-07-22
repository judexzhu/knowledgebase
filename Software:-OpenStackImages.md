# OpenStack ready images

## Overview

 * Guide to building various OpenStack ready images

## Requirements
 * Any RHEL/Fedora build with cloud-utils cloud-init cloud-utils-growpart installed 

## CentOS 
### 6.6 

+ Run the following to start the install, pulling from public mirrors 

```
virt-install --connect=qemu:///system --network=bridge:virbr0 \
--extra-args="console=tty0 console=ttyS0,115200 serial rd_NO_PLYMOUTH" \
--name=centos6.6 \
--disk path=/var/lib/libvirt/images/centos6.6.qcow2,format=qcow2,size=10,cache=none \
--ram 8192 --vcpus 6 --check-cpu --accelerate --os-type linux \
--location=http://mirror.i3d.net/pub/centos/6/os/x86_64/ --nographics
```

+ Follow the installation over console using text mode, when installation is complete log in using the previously set password and prepare the image for use with Alces Portal and OpenStack

#### Basic OpenStack ready image
+ The following packages are required to create a basic OpenStack ready CentOS 6.6 image with cloud-init and automatically resizing root partition
```
yum install -y epel-release cloud-init cloud-utils git parted
```

#### OpenStack/Alces Portal ready image
+ The following packages are required to create an OpenStack/Alces portal ready CentOS 6.6 image, with optional packages starting with the minimal requirements for an interactive desktop session 
```
# Base packages required for the OpenStack image
yum install -y epel-release \
               cloud-init \ 
               cloud-utils \ 
               git \
               parted 

# The minimal requirements for a desktop session.
yum install -y gnome-desktop \
               gnome-session \
               gnome-panel \
               nautilus \
               xorg-x11-xinit \
               urw-fonts

# Dependencies for the services tarball we deliver.
yum install -y \
    lzo \
    libyaml 

# Commonly installed utilities that we expect to be installed.
# xorg-x11-server-utils is included for xrdb.
# sysstat is included for mpstat for the compute resource monitor.
# xorg-x11-fonts-misc is included for a decent xterm font.
yum install -y \
    curl \
    fold \
    mktemp \
    tar \
    crontab \
    stat \
    gzip \
    base64 \
    tail \
    xauth \
    xlsclients \
    xterm \
    perl \
    ntp \
    xorg-x11-server-utils \
    sysstat \
    xorg-x11-fonts-misc 

# Entirely optional packages, installed here to complete compatibility
# with all of Portal's capabilities for interactive sessions.
yum install -y epel-release
yum install -y tmux
```

+ Edit `/boot/grub/menu.lst` and append the following to the kernel line to allow console access through the OpenStack Horizon dashboard
```
console=tty0 console=ttyS0,115200n8
```

+ Ensure resizing of root partition on boot
```
cd /opt
git clone https://github.com/flegmatik/linux-rootfs-resize.git
cd linux-rootfs-resize
./install
```

+ Clean up the OS and prepare ready for upload to Glance 
```
echo "NOZEROCONF=yes" >> /etc/sysconfig/network
rm -rf /etc/udev/rules.d/70-persistent-net.rules
PRIMARY_INTERFACE=$(ip route list match 0.0.0.0 | awk 'NR==1 {print $5}')
sed -i '/UUID/d' /etc/sysconfig/network-scripts/ifcfg-$PRIMARY_INTERFACE
sed -i '/HWADDR/d' /etc/sysconfig/network-scripts/ifcfg-$PRIMARY_INTERFACE
yum clean all
rm -rf /tmp/*
rm -rf /var/log/wtmp
rm -rf /var/log/btmp
history -c
shutdown -h now
```

+ Wait for the shutdown process to complete, then pack the image down using `qemu-img convert` then copy the compressed file to the required destination ready for upload to the Glance image service
```
qemu-img convert -c centos6.6.qcow2 -O qcow2 centos6.6-alces.qcow2
``` 

+ Add the new image to Glance, and optionally provide some Alces Portal metadata
```
glance image-create --name centos6.6-alces --file centos6.6.final.qcow2 --container-format bare --disk-format qcow2 --min-disk 8 --min-ram 1024 --property architecture=x86_64 --property os_distro=centos --property os_version=6.6 --property alces_process_support=interactive
```

### 7.1 

## Ubuntu
### 14.04/trusty

+ Run the following to start the install, pulling from public mirrors: 
```
virt-install --connect=qemu:///system --network=bridge:virbr0 \
--extra-args="console=tty0 console=ttyS0,115200 serial rd_NO_PLYMOUTH" \ 
--name=ubuntu1404 --disk path=/var/lib/libvirt/images/ubuntu1404.qcow2,format=qcow2,size=10,cache=none \
--ram 8192 --vcpus 6 --check-cpu --accelerate --os-type linux \
--location=http://archive.ubuntu.com/ubuntu/dists/trusty-updates/main/installer-amd64/ --nographics
```

+ Follow the installation over console using text mode, when installation is complete log in using the previously set password and prepare the image for use with Alces Portal and OpenStack

#### Basic OpenStack ready image
```
apt-get install -y cloud-init cloud-utils cloud-utils-growpart
```