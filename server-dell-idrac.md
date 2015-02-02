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
1. Disable GridEngine Queue `qmod -d *@node00`
2. Wait until node responds to `power node00 status`
3. Logon to node and run `shutdown -h 1` and wait.
4. Node probably will not turn off by itself and will probably require the power to be forced off - use the `power node00 off` command.
5. Wait 2 minutes and ensure that the node has turned off by using `power node00 status`
6. Power node back on with `power node00 on` and wait for node to boot. Note that using the `reset` power option will not fix the state!
7. Wait for node to boot, login to the node and start a `screen` session.
8. Run the BMC Firmware Update Utility - usually named something like `ESM_Firmware_3F4WV_LN_1.66.65_A00.BIN`
9. Wait for update to complete
10. Reboot node
11. Connect to node serial console `console node00`
12. Ensure that System Lifecycle Controller is not in "Recovery" status and causing boot errors.
13. Wait for node to restart
14. SSH to node and open `screen` session.
15. Run BIOS update utility - usually named something like `BIOS-xxxxx.BIN`
16. Allow system to Reboot
17. Reconnect to console after node has started rebooting 
18. Ensure Flash completes
19. Ensure Lifecycle Controller is still not in Recovery status and causing boot problems.
20. Ensure BIOS & BMC revisions are correct in `dmidecode` and by starting IPMI service and running `ipmitool mc info`
21. Re-enable queues on node `qmod -e *@node00`
