The Intel MPI Benchmark (IMB) is a suite of tests designed to performance test an MPI Interconnect either "point-to-point" of "global" for a range of message sizes. It is useful for performance testing of a cluster system, including node performance, network latency, and throughput.

### Notes for running Intel MPI Benchmark (IMB) 
* Use the `ibcheckerrors` and `ibclearerrors` to ensure that there are no issues with the fabric reported before running the tests.
* Run the IMB Benchmark one pair of nodes at a time to confirm MPI latency is as expected.
* Check /var/log/messages for any errors reported by the Operating System.
* Check the System Event Log for any reported errors.

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