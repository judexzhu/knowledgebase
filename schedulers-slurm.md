# Slurm job-scheduler

## Overview

 * This manual install method allows installation of Slurm on an EL7 cluster
 * Also tested working on an AWS Flight cluster
 * See http://slurm.schedmd.com/ for Full details

## Quick-start installation

* On all nodes:
 - sudo yum -y -e0 install epel-release
 - sudo yum -y -e0 install munge munge-devel munge-libs perl-Switch

* On master node:
 - Create munge key:
 - dd if=/dev/urandom bs=1 count=1024 > /etc/munge/munge.key
 - Copy key to all nodes into /etc/munge/munge.key
 - chmod 400 /etc/munge/munge.key (on all nodes)
 - chown munge /etc/munge/munge.key (on all nodes)
 - sudo service munge start (+chkconfig on if required)
 - TEST with "munge -n | unmunge"; benchmark with "remunge"

* on master node:
 - sudo yum install mariadb mariadb-test mariadb-libs mariadb-embedded mariadb-embedded-devel mariadb-devel mariadb-bench
 - sudo service mariadb start

* Create RPMs on master node for installation on slaves:
 - yum install perl-ExtUtils-MakeMaker readline-devel pam-devel freeipmi freeipmi-devel
 - rpmbuild -ta slurm*.tar.bz2
 - RPMs created in /root/rpmbuild/RPMS/x86_64

* Install the Slurm RPMs:
 - On master node:
 - yum -y -e0 localinstall slurm-16.05.0-1.el7.centos.x86_64.rpm slurm-devel-16.05.0-1.el7.centos.x86_64.rpm slurm-munge-16.05.0-1.el7.centos.x86_64.rpm slurm-perlapi-16.05.0-1.el7.centos.x86_64.rpm slurm-plugins-16.05.0-1.el7.centos.x86_64.rpm slurm-sjobexit-16.05.0-1.el7.centos.x86_64.rpm slurm-sjstat-16.05.0-1.el7.centos.x86_64.rpm slurm-slurmdbd-16.05.0-1.el7.centos.x86_64.rpm slurm-sql-16.05.0-1.el7.centos.x86_64.rpm slurm-torque-16.05.0-1.el7.centos.x86_64.rpm

 - On slave nodes:
 - yum -y -e0 localinstall slurm-16.05.0-1.el7.centos.x86_64.rpm slurm-devel-16.05.0-1.el7.centos.x86_64.rpm slurm-munge-16.05.0-1.el7.centos.x86_64.rpm slurm-perlapi-16.05.0-1.el7.centos.x86_64.rpm slurm-plugins-16.05.0-1.el7.centos.x86_64.rpm slurm-sjobexit-16.05.0-1.el7.centos.x86_64.rpm slurm-sjstat-16.05.0-1.el7.centos.x86_64.rpm slurm-torque-16.05.0-1.el7.centos.x86_64.rpm

* Configure slurm:
 - cd /etc/slurm/
 - cp slurm.conf.example slurm.conf
 - set cluster name and control machine, node ranges
 - "service slurm start" on all nodes
 - TEST with "scontrol show nodes" and "sinfo" from master
 - Submit simple sleeptest script with "sbatch <script>"; use "squeue" to view