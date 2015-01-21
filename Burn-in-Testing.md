# Standard Burn-In Tests for Compute Nodes.

This page gives a brief description of what burn-in tests should be run on compute node 

For standard Compute Nodes with no extra cards or hard drives:
* Run Memtester for 1 loop - Use script as provided. 
* Run iozone across system disk - Use script as provided.
* Run single round of HPL - Configure HPL.dat file as of template provided, but use HPL Calculator with correct settings.

For Compute Nodes with Infiniband card:
* Run Memtester for 1 Hour - Use script as provided. 
* Run iozone across system disk - Use script as provided.
* Run single round of HPL across nodes in configuration group (max size 32 nodes) using Interconnect. 
* Run Intel MPI Benchmark (IMB) across each pair of nodes in configuration group - Use script as provided.

For Compute Nodes with additional drives:
* Run Memtester for 1 Hour - Use script as provided. 
* Run iozone on system disk - Use script as provided.
* Run iozone on additional drives, may require manual formatting/mounting - Use script as provided. 
* Run single round of HPL across nodes in configuration group (max size 32 nodes) using Interconnect. 
* Run Intel MPI Benchmark (IMB) across each pair of nodes in configuration group - Use script as provided.

For Compute Nodes with nVidia GPGPU Cards - additionally:
* Run GPUBurn on each GPU Individually - Use script as provided.

### Notes for running Memtester
* Ensure that all System Event Logs are clear before commencing running of Memtester - you can use `service ipmi start && ipmitool -c 'sel clear'` to clear these logs.
* Ensure at least one "Loop" has been completed on each Compute Node - you can use `grep -Ri "loop" ~/memtester/output/*` to confirm this.
* Ensure no errors have been logged by Memtester during running - you can use `grep -Rci "error" ~/memtester/output/*` to confirm this.
* Ensure that the System BMC has reported no errors during running - you can use `service ipmi start && ipmitool -c sel elist` to view any reported errors from the BMC.
* Ensure that no errors have been reported by the Operating System - you can use `egrep -i "hardware|error|mce|machine"` to discount most errors that will be thrown by the OS for memory related errors.

### Notes for running HPL
* Ensure that all System Event Logs are clear before commencing running of Memtester - you can use `service ipmi start && ipmitool -c 'sel clear'` to clear these logs.
* Calculate the correct settings for the HPL.dat file with the HPL Calculator at http://hpl-calculator.sourceforge.net/ - Recommended settings are an NB size of 192 and a Memory usage of 88%.
* Use a Grid Shape (PxQ) with as close to "square" as possible, making Q slightly larger than P.
* For systems using an MPI version of 1.8.x or higher, ensure that the `mpirun` line includes the "--bind-to-core" parameter.
* Use the `ibcheckerrors` and `ibclearerrors` to ensure that there are no issues with the fabric reported before and after running the tests.

### Notes for running iozone on System Disks
* Ensure you run iozone on the disk with the biggest partition.
* Check /var/log/messages for errors with the SATA bus and/or read/write errors.

### Notes for running Intel MPI Benchmark (IMB) 
* Use the `ibcheckerrors` and `ibclearerrors` to ensure that there are no issues with the fabric reported before running the tests.
* Run the IMB Benchmark one pair of nodes at a time to confirm MPI latency is as expected.
* Check /var/log/messages for any errors reported by the Operating System.
* Check the System Event Log for any reported errors.

HPL Scripts - Example shown for 16 core, 64GB node with no IB Interconnect.

```
#!/bin/bash
# jobscript to run HPL benchmark

# Export environment and merge+set output file
#$ -j y -N HPL-1node -o /users/alces-cluster/hpl/1-node/results/HPL.$JOB_ID

# Request MPI-verbose PE 
#$ -pe mpinodes-verbose 1 -cwd -V 

module load mpi/openmpi
echo $LD_LIBRARY_PATH

ldd xhpl

# change to directory
cd /users/alces-cluster/hpl/1-node
mpirun -np 16 /users/alces-cluster/hpl/xhpl

```
HPL.dat:
```
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
2            # of problems sizes (N)
2560 81408   Ns
1            # of NBs
192          NBs
0            PMAP process mapping (0=Row-,1=Column-major)
1            # of process grids (P x Q)
4            Ps
4            Qs
16.0         threshold
1            # of panel fact
1 0 2        PFACTs (0=left, 1=Crout, 2=Right)
1            # of recursive stopping criterium
4 2          NBMINs (>= 1)
1            # of panels in recursion
2            NDIVs
1            # of recursive panel fact.
1 0 2        RFACTs (0=left, 1=Crout, 2=Right)
1            # of broadcast
0            BCASTs (0=1rg,1=1rM,2=2rg,3=2rM,4=Lng,5=LnM)
1            # of lookahead depth
0            DEPTHs (>=0)
2            SWAP (0=bin-exch,1=long,2=mix)
256          swapping threshold
1            L1 in (0=transposed,1=no-transposed) form
1            U  in (0=transposed,1=no-transposed) form
0            Equilibration (0=no,1=yes)
8            memory alignment in double (> 0)
```

IMB Script:
```
#!/bin/bash
# Export environment and merge+set output file
#$ -j y -N IMB-2nodes -o ~alces-cluster/imb/results/imb_out.$JOB_ID

# Request Parallel Environment.
#$ -pe mpinodes-verbose 2 -cwd -V

module load mpi/openmpi
module load apps/imb

# Force the MPI to use a different Interconnect by changing the btl_if_include to the name of another device.
mpirun --mca btl tcp --mca btl_tcp_if_include ib0 -np 2 -npernode 1 IMB-MPI1
```