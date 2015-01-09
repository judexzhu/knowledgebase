# Dell Power Edge C6220

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

1. Configure BMC in BIOS as per BMC options in the bios configuration
2. Configure user/password network information via ipmitool using the following parameters
  - LAN_CHANNEL=1
  - ADMIN_USER_ID=2
3. NB password for user must be at least 8 characters, contain capital letters and numbers

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

The C6220 
