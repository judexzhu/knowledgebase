# Dell PowerEdge C6320 (Haswell/Broadwell)

## Hardware Overview

 * One 2U C6000 chassis can support 2 x 2U high nodes or 4 x 1U high nodes
 * Ships with two 1200 or 1400W PSUs (single PSU is not supported)
 * Minimum of 1 node required; can ship with blanks for other node slots
 * Nodes are dual-socket Xeon E5-2600v3 or v4; supports 1 or 2 CPUs (max 135W)
 * Chassis supports up to 12 x 6TB 3.5" disks or up to 24 x 1.2TB 2.5" disks (or SSDs)
 * 16 DIMM slots; requires 1 DIMM to boot; max of 1TB RAM (16 x 64GB); slots 8-16 require CPU2 to be installed; PCB printing matches slot enumeration  

## Profile 'Cluster Slave'

***
### BIOS configuration

0. If you want to use hardware RAID0/1 on your C6320 to boot from, set that up first. 
 * Press F2 during boot to enter BIOS menu
 * Choose 'Device Settings' from the main BIOS menu
 * Choose the RAID card to configure (usually the only one listed)
 * Choose the RAID card from the next menu (usually LSI SAS2 MPT or PERC7)
 * Choose 'Controller Management'
 * Choose 'Create configuration'
 * Select your RAID level (usually RAID1 for system disks)
 * Choose 'Select Physical disks' and choose the drives to include
 * Select 'Apply changes' and confirm when prompted
 * Press ESC to exit to the main menu, saving settings when prompted
 * Continue with BIOS settings as below

1. Power on and press F2 to enter BIOS, note the firmware revisions during boot
2. Select 'System BIOS' from main menu
3. Select system defaults
```
Press TAB to move to FINISH button
LEFT arrow key to move to default and press ENTER to select
Select finish to return to main BIOS menu
```
4. From the main menu, select 'System BIOS' again.
5. Make the following changes to the default settings:

 * Choose the network interface card (NIC) you want to PXE boot from. This will either be:
      1. The onboard 10Gb SFP+ port (listed as onboard NIC XE)
   or
      2. A 1Gb port installed in the PCI mez slot (listed as the Mezzanine NIC)
The NIC you want to PXE boot from must be selected as the first boot device:
```
Boot settings -> Boot BIOS settings -> BIOS Boot sequence -> Choose NIC to PXE boot from as device 1
Boot settings -> Boot BIOS settings -> BIOS Boot sequence -> Choose boot HDD as device 2
```

If your C6320 has 1Gb network ports in and you will not be using the onboard 10Gb ports, disable them:
```
Integrated devices -> Embedded NIC 1+2 -> Disabled (OS)
```

```
Serial communication -> serial communication = ON via COM2
Serial communication -> serial port address = device1=COM1, device2=COM2
```
```
System Performance -> Select the "Performance" profile
```
```
System Security -> Set AC power recovery to "OFF"
```
Press TAB to highlight the FINISH button and press return; save and exit to main menu


6. From the main BIOS menu, select 'iDRAC settings'. Make the following change:
```
Network -> IPMI settings -> enable IPMI-over-LAN = enabled
```

Select BACK to return to the main iDRAC settings page, and select FINISH to exit, saving the changed settings.

7. From the menu BIOS menu, write down the Dell service tag printed at the bottom of the screen.
8. Select the FINISH button from the main menu, exit and reboot.


***
### Upgrading firmware

1. Three firmware payloads are required which must be compatible with each other. Firmware must be applied in the following order:
 * Fan control board (FCB); once for the C6000 chassis (run the update on the 4th node - lower right sled from front)
 * Baseboard management controller (BMC); once per node
 * BIOS; once per node

2. Other component firmware may also be required:
 * Hard disks; usually ship with recent firmware when new, but update may be needed if drives are replaced
 * NIC; usually ship with latest firmware, but may need to be updated for new features/better performance
 * Infiniband; Mellanox HBAs regularly ship with old firmware and should always be updated from Linux

3. Firmware can be applied from Linux using a BIN file, or from a bootable DOS environment. 
 * Linux firmware upgrade is safe for BIOS (ROM is loaded to BMC first then applied on the next reboot), but can damage the machine if a BMC or FCB firmware is interrupted. Take precautions to ensure that power will not be interrupted during firmware update procedure. 
 * All DOS firmware upgrades may damage the machine if interrupted while running. 

4. Download new firmware from Dell support site; www.dell.com/support

5. If using the Linux update binaries, execute the BMC and BIOS update packages on the nodes. If upgrading the FCB firmware, run this ONLY from the lower-right hand node, looking from the front. The command line switches `-f -n -q` will cause the update packages to run automatically without prompting or rebooting. BMC and FCB updates take 5-10 minutes to apply, and BIOS takes around 20 seconds to install (and a reboot to apply). 

6. If using DOS firmware update packages, boot into DOS and run the updates one by one. You don't need to reboot after each one, but reboot at the end of the process. The DOS environment can be delivered via USB boot, or via PXE boot. 

7. Each node must have power removed after doing firmware updates, and have BIOS settings reapplied. If the FCB has been upgraded, the entire chassis must be disconnected from power for at least 90 seconds. Failure to follow these suggestions will result in inconsistent behaviour, possibly many days or weeks after the updates were applied. 

### Disk drive zoning on C6000 chassis

The C6000 chassis can have a switchable SAS backplane installed, which can allow control over which disk drives are assigned to which of the nodes in the chassis. This is a rare option (as it's more expensive) and can confuse customers as drive slots do not necessarily match up to the nodes you'd expect them to. The SAS zoning utility is used to configure the backplane and assign drive slots to chassis node sled slots. Requires SAS disk drives and SAS RAID cards in the C6220 servers. It is not possible to assign the same drive slot to multiple nodes. The SAS zoning tool is available on [the Poweredge-C site](http://poweredgec.com)

***
## Hardware support

 * Call Dell on 01344-860456 with the service tag for the machine.
 * Each C6220 in a chassis has its own service tag, and the chassis has a separate tag
 * PSU faults can be opened against the chassis tag or a node tag
 * [Use this](http://creativyst.com/Doc/Articles/HT/Dell/DellNumb.htm) to convert tags to express service codes
 * Service tags are located on the rear pull handle for C6220s, and on the left hand side of the chassis for C6000

Fault finding may require a DSET report to be generated. Use the latest available DSET revision, and use "RHEL6" or "RHEL7" as the OS type. DSET will not install or run properly if the LSI MegaCLI monitor is installed, so use RPM to remove this first and YUM to install it again afterwards if necessary.

***
## Known issues
 
 * Firmware upgrade tools may randomly fail depending on versions of firmware you're upgrading from
 * Software BIOS setting tools produce inconsistent results; nodes can lose settings when disconnected from AC
 * Chassis FCB firmware may take several flashes before it starts working properly
 * Disk drive status LEDs do nothing (and sometimes light up by themselves) if no SAS RAID controller is installed
 * Differing advice on if individual nodes can be serviced without powering down entire chassis
 * Ambient temp sensor is located under front right rack ear; can lead to false overheat warning in some racks
 * Chassis is deep (>900mm) and will not fit in shallow racks with Infiniband cabling