### BIOS configuration

N.B. HP Gen9 servers ship with UEFI BIOS enabled by default. Many BIOS options (including legacy BIOS boot order) can only be configured in standard BIOS mode, so it is necessary to switch modes several times when configuring servers manually. Follow the instructions below carefully, including waiting for boot cycles to fully complete before configuring. 

 1. Power on the node with VGA screen and keyboard attached
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
Standard Boot Order (IPL) -> 2nd option -> RAID volume C:
```
```
Server availability -> ASR status -> disabled
```
```
Server availability -> AC power recovery -> Power off
```

      10. Press ESC to exit and save changes
      11. Note configuration details on asset record sheet

***

