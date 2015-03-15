# HP ProLiant BL460c G9 blade (Haswell/Broadwell Xeon CPU)

## Hardware Overview

 * Single-width half-height blade (up to 16 per chassis)
 * Nodes are dual-socket Xeon E5-2600v3; supports 1 or 2 CPUs (max 145W)
 * Blades come with or without RAID card; with RAID card supports up to 2 x 2.5" disks
 * Two onboard 10Gb ports (can run at 1Gb if a 1Gb switch is installed in chassis)
 * Has two PCI mez slots, which can take DP 10Gb or Infiniband cards
 * 16 DIMM slots; requires 1 DIMM to boot; max of 512GB RAM (16 x 32GB); slots 8-16 require CPU2 to be installed; DIMM slot enumeration printed on chassis lid underside and matches PCB printing. 135W/145W CPU support reduces number of DIMMs from 16 to 12. 
 * Ships with iLO by default (can take iLO software license upgrades)

## Profile 'Cluster Slave'

### BIOS configuration

N.B. HP BL460c Gen9 servers ship with UEFI BIOS enabled by default. Many BIOS options (including legacy BIOS boot order) can only be configured in standard BIOS mode, so it is necessary to switch modes several times when configuring servers manually. Follow the instructions below carefully, including waiting for boot cycles to fully complete before configuring. 

 1. Power on the blade with VGA screen and keyboard attached
 2. Press F9 to enter system setup
 3. Enter System BIOS, and select ```System options``` from the menu.
 4. Select ```BIOS/Platform configuration (RBSU)``` from the next menu.
 5. Press the F7 key to load defaults, and press ```Enter``` to confirm and reboot.
 6. As the node boots again, press F9 again to enter BIOS settings.
 7. At the BIOS configuration screen, select ```System options``` from the menu.
 8. Select ```BIOS/Platform configuration (RBSU)``` from the next menu.
 9. Make the following BIOS configuration settings:
```
System options -> Serial port options -> Virtual serial port -> COM2
```
```
System Options -> Processor options -> Intel Hyperthreading -> disable
```
***UP TO HERE***
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

## Hardware support

 * Call HP on 0845 161 0030 with the serial number for the machine.
 * BL460 asset tag is printed on a pull-out tag on the front of the chassis with a barcode
 
***
## Known issues
 
 * Blade chassis switches must be in correct interconnect bays in order to connect to onboard server NICs  
 * Mez card slots are not well labelled onboard the G8 blade; use the online documentation to determine which port is which
 * Servers take a long time to cold-boot; > 5 minutes with lots of PCI adapters installed
