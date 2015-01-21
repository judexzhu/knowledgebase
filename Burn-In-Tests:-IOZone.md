Iozone is useful for performing a broad filesystem analysis of a computer platform. It is a convenient way of quickly ensuring that a System Disk is performing normally. It can be used with various different options to pinpoint problems with drives, although burn-in tests, a standardised set of tests can be performed to provide a base line performance figure.

For more information about the IOZone Suite of Filesystem Benchmarks - http://www.iozone.org/docs/IOzone_msword_98.pdf

### Notes for running iozone on System Disks
* Ensure you run iozone on the system disk. Normally mounted as a "scratch" volume at `/tmp`
* Check /var/log/messages for errors with the SATA bus and/or read/write errors.

IOZone Script:
```
#!/bin/bash
#$ -pe smp 16
#$ -j y -o /users/alces-cluster/iozone/results/iozone.out.$JOB_ID

module load apps/iozone
cd /tmp
echo hostname is $HOSTNAME
iozone -a -s 8G -i0 -r 1M -d.
```