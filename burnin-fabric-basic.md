# Extended Fabric Burn-In Tests for Nodes.

This page gives a brief description of what burn-in tests should be run on nodes that have an Interconnect Card:

* Run Memtester for 1 Hour - Use script as provided. 
* Run iozone across system disk - Use script as provided.
* Run single round of HPL across nodes in configuration group (max size 32 nodes) using Interconnect. 
* Run Intel MPI Benchmark (IMB) across each pair of nodes in configuration group - Use script as provided.