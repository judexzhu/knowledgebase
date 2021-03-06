# HP Intelligent PDU

## Hardware configuration
* Available as 3-phase and single-phase base units with 16A C19 sockets on (1U or 0U mount in side of rack)
* Two types of break-out PDU available
    * Unmanaged; black sockets; 16A C19 feed
    * Managed; blue sockets; 16A C19 feed (requires special blue feed cable)
* Base unit can be ordered with PDU LCD panel which mounts on rack doors
* Unit cannot be managed without LCD panel

## Standard Base Configuration
* Get a laptop with a serial connector; setup with baud 115200, 8/N/1
* Connect to "PC" port on LCD display; note that connector on the LCD panel is female 9-pin serial
* Start minicom/HT/putty on the laptop device. The screen will not show anything when connected, even if keys are pressed. 
* EITHER power on the PDU, OR if it's already on press the RESET button on the LCD panel; resetting the LCD panel should not change the status of managed sockets, but do not perform firmware updates on other hardware while setting up the PDU
* The serial console will show startup messages (including unit's serial number); press a key when prompted to enter the menu configuration mode. If you miss the 5-second window to enter the menu, the PDU goes into lock-down mode and must be reset again. 
* Use the text menu to configure the following:
   * IPv4 IP address, netmask and gateway
   * Enable telnet server
   * Set password for the "admin" user
* When complete, go back to the main menu and press "s" to save settings and reboot
* After the PDU has rebooted, login using the web GUI and confirm settings are correct
* For managed PDUs, setup the PDU sockets in the web GUI
* Use the LCD panel buttons to show power usage on individual PDU bars and sockets. 


## Known issues

* The PDU has manual breaker trips on its top edge. When mounted in the side of the rack, these trips require equipment to be removed from the racks if they need to be accessed. 
* Individual device switching requires HP branded blue intelligent power cables between PDU and break-out bars
* HP servers and storage (including HP blade chassis with an intelligent power bar) will be auto-detected by the PDU if they are cabled using blue HP power cables. Standard power cables will supply power normally, but equipment must be setup manually. 
* The PDU's serial port goes into lock-down mode if the configuration menu is not entered within the 5-second window on boot up. This means that no output is shown on the serial console when a cable is connected after the PDU has booted, even if keys are pressed - reset the unit using the reset button to see the configuration menu. 
* PDU break-out bars and LCD screen can become unsynchronised from the main PDU unit. Reset the PDU using the button on the LCD panel if the system becomes confused. 