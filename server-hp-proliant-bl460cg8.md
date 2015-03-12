# HP ProLiant BL460c G8 blade (Sandy-bridge/Ivy-bridge)

## Hardware Overview

 * Single-width half-height blade (up to 16 per chassis)
 * Nodes are dual-socket Xeon E5-2600 or E5-2600v2; supports 1 or 2 CPUs (max 135W)
 * Blades come with or without RAID card; with RAID card supports up to 2 x 2.5" disks
 * Two onboard 10Gb ports (can run at 1Gb if a 1Gb switch is installed in chassis)
 * Has two PCI mez slots, which can take DP 10Gb or Infiniband cards
 * 24 DIMM slots; requires 1 DIMM to boot; max of 768GB RAM (24 x 32GB); slots 8-16 require CPU2 to be installed; DIMM slot enumeration printed on chassis lid underside and matches PCB printing 
 * Ships with iLO by default (can take iLO software license upgrades)

## Profile 'Cluster Slave'

### RAID disk configuration
If your node has RAID disks, follow these instructions to configure them:
1. Power on the node with a VGA screen and keyboard attached.
2. At P220 RAID card prompt, press F8 to configure
3. Delete any logical drives configured. 
4. Create a new logical drive; select RAID0 or RAID10 for the two disks
5. Choose "select boot volume" from the RAID card main menu
6. Select LUN0 (the logical drive created in step 4)
7. Press ESC at the main menu to exit

N.B. If you have created a new logical drive, you will need to power cycle the node before you can select it in the BIOS boot order. 

### BIOS configuration

1. Power on the blade with VGA screen and keyboard attached
2. Press F9 to enter system setup
3. Enter System BIOS and configure the following:
```
System Default menu -> System default options: Confirm
```
The node will reboot automatically. 
4. As the node boots again, press F9 again to enter BIOS settings.
5. At the BIOS configuration screen, make these settings:
```
System Information -> Note down Firmware Revisions for BIOS and serial number for the blade
```
```
System options -> Processor options -> Intel HT Options -> Disable hyperthreading
```
```
System options -> Embedded NICs -> NIC2 boot options -> Select "Network boot PXE"
```
```
System options -> Serial port options -> Virtual serial port -> COM2
```
```
System options -> BIOS serial console & EMS -> Port -> COM2
System options -> BIOS serial console & EMS -> Baud -> 115200
System options -> BIOS serial console & EMS -> EMS -> COM2
```
```
Power Management -> HP Power Profile -> Select "Maximum performance"
```
```
Standard Boot Order (IPL) -> 1st option -> NIC1
Standard Boot Order (IPL) -> 2nd option -> P220i RAID volume C:
```
```
Server availability -> ASR status -> disabled
```
```
Server availability -> Automatic power on -> disabled
```
6. Press ESC to exit and save changes
7. Note configuration details on asset record sheet

***
### Upgrading firmware

1. Two primary firmware payloads are required which must be compatible with each other. Firmware must be applied in the following order:
 * iDRAC firmware; can be upgraded from Linux without restarting node
 * BIOS; new firmware is uploaded to iDRAC and applied by lifecycle manager on the next reboot. Failsafe firmware ROM automatically stores backup copy of firmware. 

2. Other component firmware may also be required:
 * Hard disks; usually ship with recent firmware when new, but update may be needed if drives are replaced
 * NIC; usually ship with latest firmware, but may need to be updated for new features/better performance
 * PERC (RAID card); usually ships on latest version, but may need update for older systems
 * Infiniband; Mellanox HBAs regularly ship with old firmware and should always be updated from Linux

3. Firmware can be applied from Linux using a BIN file, or from a bootable DOS environment. 
 * Linux firmware upgrade is safe for BIOS (ROM is loaded to BMC first then applied on the next reboot), but can damage the machine if an iDRAC firmware is interrupted. Take precautions to ensure that power will not be interrupted during firmware update procedure. 
 * All DOS firmware upgrades may damage the machine if interrupted while running. 

4. Download new firmware from Dell support site. 

5. If using the Linux update binaries, execute the iDRAC and BIOS update packages on the nodes. The command line switches `-f -n -q` will cause the update packages to run automatically without prompting or rebooting. iDRAC updates can take up to 30 minutes to apply, and BIOS takes around 20 seconds to install (and a reboot to apply). 

6. If using DOS firmware update packages, boot into DOS and run the updates one by one. You don't need to reboot after each one, but reboot at the end of the process. The DOS environment can be delivered via USB boot, or via PXE boot. 

7. BIOS settings usually persist across firmware updates 

***
### Configuring BIOS and iDRAC settings 

Dell provides tools to perform BIOS and BMC configuration settings from Linux. The tools often produce inconsistent behaviour when setting the configuration (depending on the installed firmware version), but are generally fairly reliable for reading the current settings in place from BIOS. 

 * Tools are available from the Dell support site in the "DTK" package
 * Most successful version historically has been "dtk_4.3_350_A00_Linux64.iso"
 * Install the following packages ONLY:
    * syscfg-4.3.0-4.33.4.el6.x86_64.rpm 
    * srvadmin-deng-7.3.0-4.13.2.el6.x86_64.rpm 
    * srvadmin-hapi-7.3.0-4.12.3.el6.x86_64.rpm 
    * srvadmin-isvc-7.3.0-4.21.4.el6.x86_64.rpm 
    * srvadmin-omilcore-7.3.0-4.72.1.el6.x86_64.rpm
 * Once installed, start the firmware management service with:
```
/opt/dell/srvadmin/sbin/srvadmin-services.sh start
```
 * Use the syscfg utility to view and change BIOS and iDRAC settings
```
/opt/dell/toolkit/bin/syscfg
```

***
### RAID disks and storage

The R630 can be supplied with:

 * No RAID controller; supports SATA disks or SSDs, or no disks at all. This configuration required no hardware setup. SAS disks are not supported without a RAID controller. RAID controller slot is empty but cannot be used for anything else. 
 * R630 supports PCI-express SSDs mounted in the front disk bays. These do not require a RAID card. 
 * PERC H330 SAS controller in onboard RAID controller slot; Supports RAID0+1 only - see hardware asset record for RAID setup info.
 * PERC H730 SAS controller in onboard RAID controller slot; includes battery (in dedicated internal slot) and 1GB RAM - PERC H730P controller has 2GB RAM; Supports RAID 0,1,5,6 - see hardware asset record for RAID setup info.
 * Requires Nagios SMART disk monitor is no RAID card is installed. 
 * Requires Nagios RAID monitor if SAS RAID card is installed. 

***
### Compatible devices

R630 always ships with one of the following NIC daughercards:
 - 4 x 1Gb RJ45 ports, Intel or broadcom
 - 2 x 10Gb SFP+ ports and 2 x 1Gb RJ45 ports, Intel or Broadcom
 - 2 x 10Gb RJ45 and 2 x 1Gb RJ45 ports, Intel or Broadcome

All ports can be configured for PXE and iSCSI boot, and iDRAC can share any port, on the base or a tagged VLAN. 

The following list of hardware is certified for installation:
 * Mellanox QDR and FDB, single and dual-port PCI-express and mez HCA; FDR cards can be factory installed
 * Intel/Qlogic TrueScale QDR Infiniband PCI-express and mez HCA
 * Intel and Broadcom, dual-port 10Gb SFP+ and 10G-baseT (copper) PCI-express and mez cards
 * Quad-port Intel gigabit NIC 
 * 12Gb and 6Gb SAS HBA, FC cards, PERC H830 external RAID card

Some simple rules for installing add-on cards:
 * The 3-PCI slot version has low-profile, half-length slots. Check physical card dimensions before installing
 * In the 2-PCI slot version, the second PCI-express riser does not ship with systems by default; order the riser if you need to fit two PCI cards
 * The system has a dedicated internal RAID card slot which the PERC H330/H730 cards fit into
 * Cards in PCI slots receive hot air from CPU and RAM; check PCI card tolerances before installing

***
## Hardware support

 * Call Dell on 01344-860456 with the service tag for the machine.
 * R630 asset tag is printed on a pull-out tag on the front of the chassis with a QR code
 * [Use this](http://creativyst.com/Doc/Articles/HT/Dell/DellNumb.htm) to convert tags to express service codes
 
Fault finding may require a DSET report to be generated. Use the latest available DSET revision, and use "RHEL6" or "RHEL7" as the OS type. DSET will not install or run properly if the LSI MegaCLI monitor is installed, so use RPM to remove this first and YUM to install it again afterwards if necessary.

***
## Known issues
 
 * Firmware upgrade tools may fail to run without various Linux dependencies 
 * Software BIOS setting tools produce inconsistent results 
 * Older iDRAC firmware had memory leaks which caused the entire server to fail after several months of running; this is fixed in recent versions, but the bug has been reintroduced more than once in the past
 * Disks, PSUs and fans are technically hot-swappable (although difficult to reach fans when powered)
 * iDRAC processors can ship with SD cards (called Vflash) which presents as /dev/sda in Linux, causing problems installing an OS; remove these cards if installed from the rear of the chassis, or internally from both sides of the PCI slot riser closest to the PSUs
 * IPMI sensors will fail to detect redundant PSUs properly if iDRAC is not set to "redundant" power; if this happens, login to the iDRAC using a web browser and check the power settings. Older firmware had a bug whereby PSU redundancy would change by itself after many months of running. Mode sometimes needs to be manually set to non-redundant and saved, then set back to redundant mode again before ipmi-sensors will report properly. 