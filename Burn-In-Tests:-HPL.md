High Performance Linpack (HPL) is a synthetic benchmark, designed to solve a large mathematical problem on computers - including those with Distributed Memory, such as those within a cluster. 

It solves the problem in Double-Precision (64bit) arithmetic, which quickly stresses the Floating Point unit of a CPU along with the Memory subsystem and will normally show any hardware problems with a node quite quickly. When run across multiple nodes via an interconnect, it is useful for showing up errors on the fabric as poor performance will be exhibited and often accompanied with reported errors.

### Notes for running HPL
* Ensure that all System Event Logs are clear before commencing running of Memtester - you can use `service ipmi start && ipmitool -c 'sel clear'` to clear these logs.
* Ensure that the `ibcheckerrors` and `ibclearerrors` commands are used to check for any current errors and clear any previous errors reported on the Fabric.
* Calculate the correct settings for the HPL.dat file with the HPL Calculator at http://hpl-calculator.sourceforge.net/ - Recommended settings are an NB size of 192 and a Memory usage of 88%.
* Use a Grid Shape (PxQ) with as close to "square" as possible, making Q slightly larger than P.
* For systems using an MPI version of 1.8.x or higher, ensure that the `mpirun` line includes the "--bind-to-core" parameter.
* Use the `ibcheckerrors` command to ensure that there are no issues with the fabric reported after running the HPL benchmark.

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