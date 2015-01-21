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