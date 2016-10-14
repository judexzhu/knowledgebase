# Dell Power Edge FC430 (Haswell/Broadwell) 

## Hardware Overview

 * Quarter-U node with optional front-facing Infiniband mez card
 * Fits in Dell FX chassis (either with or without rear PCI slots)
 * Has access to one PCI slot in relevant chassis
 * Nodes are dual-socket Xeon E5-2600v3 or v4
 * One SSD (with front-facing IB mez) or two SSDs (1.8" size)
 * 8 DIMM slots; requires 1 DIMM to boot; max of 512GB RAM (8 x 64GB); slots 5-8 require CPU2 to be installed

## Profile 'Cluster Slave'

***
### BIOS configuration
1. Power on and press F2 to enter BIOS, note the firmware revisions during boot
2. Enter first pass BIOS and configure the following:
```
System BIOS -> Load Optimal Defaults -> “yes”
Save settings and exit
Re-enter System BIOS menu
```
```
Processor settings -> Disable logical processor (Hyperthreading)
```
```
System Boot settings -> BIOS Boot settings -> Boot sequence
 - Set the following sequence:
    - Embedded NIC 1 port 1
    - Hard drive C:
    - (any other entries)
```
```
Serial communication menu:
Serial communication -> On with console redirection
Serial port address -> COM2
Re-direction after boot -> Disabled
```
```
System profile settings -> System profile -> Performance
```
```
System security -> AC power recovery -> OFF
```
```
Miscellaneous settings -> F1 prompt for keyboard error -> Disabled
```
Save settings and exit the BIOS settings menu.
Enter the iDRAC settings menu
```
Network -> Enable IPMI over LAN -> Enabled
```
```
Thermal -> Maximum performance
```
```
Exit -> Save Settings and Exit
```

3. Note configuration details on asset record sheet

***
### BMC configuration

 1. Configure BMC in BIOS as per BMC options in the BIOS configuration (above)
 2. Configure user/password network information via ipmitool using the following parameters
``` 
 LAN_CHANNEL=1
 ADMIN_USER_ID=2 
``` 
 3. N.B. password for user must be at least 8 characters, contain capital letters and numbers
 4. BMC serial console is connected to ttyS1 (COM2) speed 115,200 parity 8n1. 

***

## Known issues
 
 * FX chassis CMC IP address may need setting manually; this can be accessed from the iDRAC menu, under the "CMC Network menu
