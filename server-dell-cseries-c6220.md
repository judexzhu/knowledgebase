# Dell Power Edge C6220 (Sandy-bridge) and C6220-II (Ivy-Bridge)

## Profile 'Cluster Slave'

### BIOS
1. Power on and press F2 to enter BIOS, note the firmware revisions during boot
2. Enter first pass BIOS and configure the following:
```
Exit -> Load Optimal Defaults -> “yes”
```
```
Advanced -> Power Management -> “Maximum Performance”
```
```
Advanced -> CPU Configuration -> C­States -> “Disabled”
```
```
Advanced -> CPU Configuration -> Hyperthreading/SMT -> “off”
```
```
Advanced -> CPU Configuration -> Turbo Mode -> “On”
```
```
Boot -> Quiet Boot -> “Disabled”
```
```
Server -> Set BMC LAN Configuration -> BMC LAN Port Configuration “Dedicated-NIC”
```
```
Server -> Restore on AC Power Loss -> “Always Off”
```
```
Exit -> Save Settings and Exit
```
3. Note configuration details on asset record sheet

### BMC

1. Configure BMC in BIOS as per BMC options in the BIOS configuration (above)
2. Configure user/password network information via ipmitool using the following parameters
  - LAN_CHANNEL=1
  - ADMIN_USER_ID=2
3. N.B. password for user must be at least 8 characters, contain capital letters and numbers

### Upgrading firmware

1. Three firmware payloads are required which must be compatible with each other. Firmware must be applied in the following order:
 * Fan control board (FCB); once for the C6000 chassis (run the update on the 4th node - lower right sled from front)
 * Baseboard management controller (BMC); once per node
 * BIOS; once per node

Other component firmware may also be required:
 * Hard disks; usually ship with recent firmware when new, but update may be needed if drives are replaced
 * NIC; usually ship with latest firmware, but may need to be updated for new features/better performance
 * Infiniband; Mellanox HBAs regularly ship with old firmware and should always be updated from Linux

2. Firmware can be applied from Linux using a BIN file, or from a bootable DOS environment. 
 * Linux firmware upgrade is safe for BIOS (ROM is loaded to BMC first then applied on the next reboot), but can damage the machine if a BMC or FCB firmware is interrupted. Take precautions to ensure that power will not be interrupted during firmware update procedure. 
 * All DOS firmware upgrades may damage the machine if interrupted while running. 

3. Download new firmware from Dell support site. Missing versions may be available on http://poweredgec.com

4. If using the Linux update binaries, execute the BMC and BIOS update packages on the nodes. If upgrading the FCB firmware, run this ONLY from the lower-right hand node, looking from the front. The command line switches "-f -n -q" will cause the update packages to run automatically without prompting or rebooting. BMC and FCB updates take 5-10 minutes to apply, and BIOS takes around 20 seconds to install (and a reboot to apply). 

5. If using DOS firmware update packages, boot into DOS and run the updates one by one. You don't need to reboot after each one, but reboot at the end of the process. The DOS environment can be delivered via USB boot, or via PXE boot. 

6. Each node must have power removed after doing firmware updates, and have BIOS settings reapplied. If the FCB has been upgraded, the entire chassis must be disconnected from power for at least 90 seconds. Failure to follow these suggestions will result in inconsistent behaviour, possibly many days or weeks after the updates were applied. 

### Configuring BIOS and BMC settings 

Dell provides tools to perform BIOS and BMC configuration settings from Linux. The tools often produce inconsistent behaviour when setting the configuration (depending on the installed firmware version), but are generally fairly reliable for reading the current settings in place from BIOS. 

 * Tools are available from http://poweredgec.com/
 * Use TGZ packages only; the RPM versions conflict with other packages in RHEL distributions
 * "setupbios" utility allows BIOS settings to be captured and set individually (but not reliably)
 * "bmc" utility allows BMC settings to be captured, and some options to be set (although ipmitool is generally more reliable)

### RAID disks and storage

The C6220 can be supplied with:

 * No RAID controller; includes SATA disks or SSDs, or no disks at all. This configuration required no hardware setup. SAS disks are not supported without a RAID controller. 
 * LSI 2008 SAS controller in onboard mez slot; press CTRL+C to configure in BIOS; use the mpt2sas driver and sas2ir configuration tool in Linux. Supports RAID0+1 only - see hardware asset record for RAID setup info.
 * LSI 9260 SAS controller in PCI slot; battery must be installed on riser at sled front (reject node if battery is installed on RAID card itself, as it will overheat); press CTRL+R on startup to configure.  Supports RAID 0,1,5,6 - see hardware asset record for RAID setup info.
 * Requires Nagios SMART disk monitor is no RAID card is installed. 
 * Requires Nagios RAID monitor if SAS RAID card is installed. 

### Disk drive zoning on C6000 chassis

The C6000 chassis can have a switchable SAS backplane installed, which can allow control over which disk drives are assigned to which of the nodes in the chassis. This is a rare option (as it's quite expensive) and can confuse customers as drive slots do not necessarily match up to the nodes you'd expect them to. The SAS zoning utility is used to configure the backplane and assign drive slots to chassis node sled slots. Requires SAS disk drives and SAS RAID cards in the C6220 servers. It is not possible to assign the same drive slot to multiple nodes. The SAS zoning tool is available on http://poweredgec.com

### Compatible devices

The following list of hardware is certified for installation into a C6220:
 * Mellanox QDR and FDB, single and dual-port PCI-express and mez HCA
 * Intel/Qlogic TrueScale QDR Infiniband PCI-express and mez HCA
 * Intel and Broadcom, dual-port 10Gb SFP+ and 10G-baseT (copper) PCI-express and mez cards
 * Quad-port Intel gigabit NIC 
 * Nvidia PCI-express host interface card (HIC) for connection to external PCI chassis (C410X)

Some simple rules for installing add-on cards:
 * The PCI-express slot is low-profile, half-length. Check physical card dimensions before installing
 * The PCI-express riser does not ship with systems by default; order the riser if you need to fit PCI cards
 * The mez slot is mechanically different to previous Dell servers (C6100), so cards are not compatible
 * Cards in PCI and mez slots receive hot air from CPU and RAM; check PCI card tolerances before installing

## Hardware support

 * Call Dell on 01344-860456 with the service tag for the machine.
 * Each C6220 in a chassis has its own service tag, and the chassis has a separate tag
 * PSU faults can be opened against the chassis tag or a node tag
 * [Use this](http://creativyst.com/Doc/Articles/HT/Dell/DellNumb.htm) to convert tags to express service codes
 * Service tags are located on the rear pull handle for C6220s, and on the left hand side of the chassis for C6000

Fault finding may require a DSET report to be generated. Use the latest available DSET revision, and use "RHEL6" or "RHEL7" as the OS type. DSET will not install or run properly if the LSI MegaCLI monitor is installed, so use RPM to remove this first and YUM to install it again afterwards if necessary.