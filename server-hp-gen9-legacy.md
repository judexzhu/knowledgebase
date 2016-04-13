## HP Gen9 Legacy-BIOS configuration

### Info
 * HP servers ship with UEFI BIOS enabled by default
 * Legacy-BIOS must be enabled and the server rebooted before settings are available
 * Legacy BIOS POST is slower than UEFI-BIOS POST


***
### Standard BIOS setting configuration
 1. Power on the node with VGA screen and keyboard attached
 2. Press F9 to enter system setup
 3. Select the ```System Configuration``` menu
 4. Select ```BIOS/Platform configuration (RBSU)``` from the next menu.
 5. Press the F7 key to load defaults, and press ```Enter``` to confirm and reboot.
 6. As the node boots again, press F9 again to enter BIOS settings.
 7. Select the ```System Configuration``` menu
 8. Select ```BIOS/Platform configuration (RBSU)``` from the next menu.
 9. Select ```Boot options```, then the ```Boot Mode``` option
 10. Select ```Legacy BIOS mode``` - press F10 to save, then reboot the server
 11. As the node boots again, press F9 again to enter BIOS settings.
 12. Select the ```System Configuration``` menu
 13. Select ```BIOS/Platform configuration (RBSU)``` from the next menu.
 14. Make the following BIOS configuration settings:
```
System options -> Processor option -> Hyperthreading -> [disable|enable]
```
Enable hypertheading on machines which will be running VMs; disable it on everything else. 
```
System Options -> SATA controller options -> Embedded SATA configuration -> Enable SATA AHCI support
```
```
Power Management -> Power profile -> Maximum performance
```
```
Server availability -> automatic power-on -> always-off
```
Master nodes should be configured to automatically power on after a 60-second delay.

  10. Press ESC to exit and press "Y" to save changes
  11. If the server has a RAID disk, you need to set it up now so it can be seen as a valid legacy boot option. Select the RAID card from the BIOS main menu to configure it.
  12. Once any RAID cards have been configured, reboot the server. 
  15. As the server reboots, press F9 again to enter BIOS settings
  16. Select the ```System Configuration``` menu
  17. Select ```BIOS/Platform configuration (RBSU)``` from the next menu.
  18. Select the ```Boot options``` menu
  19. Select the ```Legacy BIOS boot order``` menu
  20. Use the "+" and "-" keys to rearrange the boot order:
      - Boot first from the main PXE device (usually onboard 1Gb port 1)
      - Boot second from the main hard disk or RAID device
  21. When complete, press F10 to save and press "Y" to confirm
  22. Press ESC until you return to the main menu
  23. Select the ```iLO4 configuration``` configuration menu
  24. Select the ```Network Options``` menu
  25. Set the ```Network Interface adapter``` option appropriately:
       * If the server has a dedicated iLO network port, use the "ON" setting
       * If the server does not have a dedicated iLO port, selecting sharing with one of the network ports
       * For Apollo2000 and 6000 servers that have a shared chassis iLO port (or RSM adapter), select the "ON" setting
  26. Press F10 to save settings; when prompted, press "Y" to confirm, then press "ENTER" ONLY to confirm that the iLO will be rebooted (otherwise the settings will not be saved)
  27. Wait for 30 seconds for iLO to reboot
  28. Reboot the server 

***
### Known Issues
 * If the iLO IP address is set from the OS, the iLO may not be contactable over the network until it's been rebooted. Use "ipmitool mc reset cold" on the server from Linux to force the iLO to reset
 * The B140i (SW) RAID controller needs a Linux driver to work in RAID1 mode, and does not support BIOS legacy boot; use UEFI boot and a customised Linux installation image to use this card in Linux