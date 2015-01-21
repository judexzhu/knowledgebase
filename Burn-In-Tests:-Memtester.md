Memtester is a utility for testing the memory of a node, it is run one process per CPU core with the memory split evenly between each CPU core - it is run until generally one entire loop has been completed (i.e. all the memory in a system has been tested by each core). Some notes and scripts are provided below to provide reference when performing this test:

### Notes for running Memtester
* Ensure that all System Event Logs are clear before commencing running of Memtester - you can use `service ipmi start && ipmitool -c 'sel clear'` to clear these logs.
* Ensure at least one "Loop" has been completed on each Compute Node - you can use `grep -Ri "loop" ~/memtester/output/*` to confirm this.
* Ensure no errors have been logged by Memtester during running - you can use `grep -Rci "error" ~/memtester/output/*` to confirm this.
* Ensure that the System BMC has reported no errors during running - you can use `service ipmi start && ipmitool -c sel elist` to view any reported errors from the BMC.
* Ensure that no errors have been reported by the Operating System - you can use `egrep -i "hardware|error|mce|machine"` to discount most errors that will be thrown by the OS for memory related errors.

Memtester Example Job Script:
```
#!/bin/bash
#$ -N Memtest -pe smp 16
#$ -j y -o /users/alces-cluster/memtest/results/memtest.out.$JOB_ID
LIMIT=`grep -c processor /proc/cpuinfo`
RUN=0;
COUNT=0

module load apps/memtester

totfree=`free -m  | grep Mem | awk '{print $4}'`
free=`expr $totfree - 256`
RAMperjob=`expr $free \/ $LIMIT`

echo "Hostname is `hostname`"
#Start a memtester for each CPU, we assume the machine has $RAMperjob MB RAM per core
echo "Starting memtest runs at `date`"
while [ $COUNT -lt $LIMIT ]; do
  memtester ${RAMperjob}M >> /tmp/memtest.$COUNT.out 2>&1 &
  let COUNT+=1
done

echo "Memtest started ($LIMIT copies - infinite runtime) - monitoring system load"
while true
do
   uptime
   sleep 300
done
```