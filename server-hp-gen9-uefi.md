## HP Gen9 UEFI BIOS configuration

### Info
 * HP servers ship with UEFI BIOS enabled by default
 * UEFI requires special PXE and GRUB settings in your OS
 * UEFI allows some special features including:
     * PXE boot from tagged VLAN
     * Booting HP systems from onboard B110i (SW) RAID controller
 * UEFI POST is much faster than legacy-BIOS POST



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
 9. Make the following BIOS configuration settings:
```
System options -> Processor option -> Hyperthreading -> [disable|enable]
```
Enable hypertheading on machines which will be running VMs; disable it on everything else. 
```
System Options -> SATA controller options -> Embedded SATA configuration -> Enable SATA AHCI support
```
For systems with two HDDs to be configured in RAID1 with B140i controller, SATA RAID must be enabled. This option only works with UEFI boot and requires an OS driver to be loaded during OS install to work. 
```
Power Management -> Power profile -> Maximum performance
```
```
Server availability -> automatic power-on -> always-off
```
Master nodes should be configured to automatically power on after a 60-second delay.


  10. Press ESC to exit and press "Y" to save changes
  11. If the server has a hardware RAID card, return to the main menu and select the RAID card from the configuration menu. Select "Launch HPSA" to start the RAID configuration screen.
  12. Allow the RAID configuration screen to start; find a USB mouse and use it to configure the RAID card.
  13. If you create a new RAID disk to boot from, remember to set it as a boot device. 
  14. When RAID is configured, click on the icon in the top right-hand corner to exit and reboot. The reboot button might require clicking on several times. 
  15. Press F9 again to enter BIOS settings
  16. Select the ```System Configuration``` menu
  17. Select ```BIOS/Platform configuration (RBSU)``` from the next menu.
  18. Select the ```Boot options``` menu
  19. Select the ```UEFI boot order``` menu
  20. Use the "+" and "-" keys to rearrange the boot order
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