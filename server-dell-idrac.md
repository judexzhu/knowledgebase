# Dell iDRAC


***
### Known issues
* There were a number of bad firmware payloads for Dell iDRACs which caused them to lock-up after several 100 days of running. In extreme cases, the iDRAC would also prevent the server running properly. A server in this state will often show odd CPU runtimes in top; e.g. 
```
[root@node07(tde-hpc1) ~]# top
top - 16:28:23 up  4:22,  1 user,  load average: 0.03, 0.01, 0.00
Tasks: 373 total,   1 running, 372 sleeping,   0 stopped,   0 zombie
Cpu(s):  0.0%us,  0.0%sy,  0.0%ni,100.0%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
Mem:  132112272k total,  1201896k used, 130910376k free,    35908k buffers
Swap: 33554424k total,        0k used, 33554424k free,   215336k cached

  PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND                                                                                                                                                                                                           
 8035 postfix   20   0 77684 3244 2400 S 100.0  0.0  1741127h pickup
 8884 root      20   0  107m 4176 3148 S 100.0  0.0  5139105h sshd
 8973 root      20   0 13436 1404  876 R 100.0  0.0  82982,17 top
    1 root      20   0 25696 1636 1292 S  0.0  0.0  9256388h init
    2 root      20   0     0    0    0 S  0.0  0.0 750485,50 kthreadd
```

The process for fixing this is as follows:
1. One
2. Two
3. Three