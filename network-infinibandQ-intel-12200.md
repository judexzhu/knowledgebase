# Intel TrueScale 12300 and 12200 switch 
## aka Qlogic TrueScale QL12300 and 12200

## Hardware overview
* 36-port QDR Infiniband 1U switch (although an 18-port model called 12300-BS18 was also available)
* Several different versions:
  * 12300 switch has hot-swap PSUs, while 12200 model has fixed PSUs
  * -BS01 model has a single PSU, standard airflow (inlet over flat side, exhaust over ports)
  * -BS23 model has dual PSUs, standard airflow (inlet over flat side, exhaust over ports)
  * -BS22 has dual PSUs and reversed airflow (inlet over ports, exhaust over flat side)
* All 12300 models include subnet management card 
* 12200 models available with an optional subnet management card 
  * If 12200 part number has "-MM" tail if management card is installed; e.g. 12200-BS23-MM
* All switches have both a serial port and an Ethernet port; Ethernet port only works if switch is managed
* Last few Qlogic branded switches were being shipped during 2014

## Standard Base Configuration profile: 'Cluster Basic'

### Unmanaged switches
* No configuration required (or possible)

### Managed switches
* Apply power to switch
* Connect to the switch via serial console  (57600 baud, 8/n/1)
* If the switch has already been configured with an IP address, use telnet to connect (username *admin*, password *adminpass*)
* If the backspace key isn't working, switch to the alternative key using the command:
```
swapBsDel
```
* To set an IP address, use the command:
```
setChassisIpAddr -h <ipaddress> -m <netMask>
setDefaultRoute -h <ipaddress>

```
* Set the system name:
```
setSystemName
```
* Set the username and password (default old password: adminpass)
```
passwd 
```
* Power down the switch, then power it back up after 60 seconds and check the settings have applied.
* Record details on customer asset sheet
 - Asset Tag / Serial Number
 - Firmware revision
 - Name
 - IP Details of management interface
 - Username/password
 - Any settings applied that are not part of the standard configuration above

***

## Firmware update
* Switches usually ship on latest firmware. 
* Note that Intel Infiniband QDR host channel adapter PCI-cards do not have firmware; their code is loaded on boot via the kernel driver. This is different to DDR Qlogic Infiniband cards, which were OEM from Mellanox, so do have firmware on them which can be upgraded. 

### Switches with management module installed
* Upgrading the firmware is simple to do via web-based GUI
* Upgrade requires the switch to be configured with an IP address
* Ensure all switches in the fabric are running the same firmware

### Switches without management module
* Firmware is extremely basic, and only really handles electrical negotiation with cables and other switches. If you're hoping that a firmware upgrade will fix your MPI problem, look elsewhere
* New firmware files can be downloaded from Intel support page
* Use the Intel "iba_manage_switch" utility to down new firmware to switches
   * You will need the GUID of the switch (e.g. `0x066a22e30028ac`) to upgrade it; use the ***ibswitches*** command to find it
   * Check the existing firmware image before updating:
```
iba_manage_switch -t 0x066a22e30028ac showFwVersion
```
   * Copy the firmware file (using the original filename - .emfw extension) to your working directory, and run:
```
iba_manage_switch -t 0x066a22e30028ac -f Intel_12000_V1_firmware.7.3.0.3.2.emfw fwUpdate
```
   * Check the new firmware image has installed properly 
```
iba_manage_switch -t 0x066a22e30028ac showFwVersion
```

***
## Hardware support
* Email Intel ibsupport@intel.com with the switch serial number
* Serial numbers are printed on switch chassis; labels may be stuck almost anywhere on the outside (including the sides where they're covered by the rack rails)
* Serial numbers are NOT the same as the GUID:
   * A GUID typically looks like this: 0x00066a00e3001234
   * Intel serial numbers typically look like this: USF1410920002
   * Serial number can be retrieved if you have console access by using the command `fruinfo`

***
## Known issues
* If a PSU fails, the switch must be physically replaced
* Various issues negotiating with Mellanox switches; try not to mix switch types on the same fabric
* Max supported copper cable length is 5M
* Max supported optical cable length is 30M
* In-band firmware management tools (for unmanaged switches) can deliver inconsistent results under certain operating systems; the switches contain redundant ROMs and will reboot to safe mode if a firmware update fails. 