# HP Networking 2920
## 48-port 1Gb Ethernet switch

## Hardware overview
* 48-port gigabit-Ethernet switch with 4 x SFP+ 1Gb module slots
* Single PSU + DC power socket for external PSU
* Port-to-PSU airflow by default
* 1Gb ports do not support direct-attach (copper/twinAX) cables

## Standard Base Configuration

* Connect to the switch via serial console  (9600 baud, 0/n/1)
* Apply power to switch
* Wait for the console to prompt - press return when "speed detect" is shown
* Press return to log into the switch
* At the prompt, type "setup"
* Follow the menu to setup the hostname, IP address, password and enable spanning-tree
* Select "Save" when complete

## Saving switch details
* Record details on customer asset sheet
* From the menu system, select "General information"
* From the CLI, type "menu" to enter the menu system
 - Serial Number
 - Firmware revision
 - Name
 - IP Details of management interface
 - Username/password
 - Any settings applied that are not part of the standard configuration above

***
## Known issues
* Configuration made in the menu automatically saves once entered; configuration made in the CLI requires "write mem" command to save, otherwise settings are lost on a reboot
* Does not support copper SFP+ DAC cables
* Redundant PSU; cables, PSUs and PSU-chassis must all be ordered and shipped separately; ordering any individual part is useless without the others