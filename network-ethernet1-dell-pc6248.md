# Dell PowerConnect 6248 48-port
## and 6224 24-port switches

## Hardware overview
* 48-port gigabit-Ethernet switch with 2 x module slots
* 24-port version is called 6224 and also has 2 x module slots
* Single internal PSU, with DC socket for external PSU
* Side-to-side airflow
* Modules include 2 x 10Gb SFP+, 2 x CX4 stacking ports, 2 x copper 10G-baseT ports

## Standard Base Configuration

* Connect to the switch via serial console  (9600 baud, 0/n/1)
* Apply power to switch
* Wait for the console to prompt the setup wizard, and press “Y” for yes within 60 seconds
* When prompted to setup SNMP, press No
* When prompted, press Yes to configure IP address, Subnet Mask and Gateway – use as provided by preinstall or otherwise instructed for the cluster
* When prompted, set the username to “admin” and the password to the supplied cluster admin/IPMI password
* In your browser connect to the IP address nominated for the switch.

* Apply the following settings via the management GUI:
```
System settings -> General -> Asset -> Set the system name to the name of the switch
```
```
File Management -> Copy Files -> Copy Running Config to Startup Config
```
* Power down the switch, then power it back up after 60 seconds and check the settings have applied.

## **Cluster simple** profile
![draft](http://upload.wikimedia.org/wikipedia/commons/f/ff/DRAFT_ICON.png)
 * Basic setup, no VLANs
 * Connect to the switch management GUI via the IP address set above
```
Switching -> Spanning Tree -> General Settings -> Disable Spanning Tree
```
```
File Management -> Copy Files -> Copy Running Config to Startup Config
```
* Power down the switch, then power it back up after 60 seconds and check the settings have applied.

## **Cluster VLAN** profile
![draft](http://upload.wikimedia.org/wikipedia/commons/f/ff/DRAFT_ICON.png)
 * Configuration for modern clusters and private clouds
 * Rapid spanning-tree (RSTP) enabled plus default VLANs:
    *  VLAN1 = hardware mgt (BMC, switch interface, etc.)
    *  VLAN2 = private network (blue)
    *  VLAN3 = software mgt / build network (PXE boot, ganglia, nagios)
    *  VLAN4 = DMZ network
    *  VLAN5 = customer external network	

 * Connect to the switch management GUI via the IP address set above
```
Switching -> Spanning Tree -> General Settings -> Enable Rapid Spanning Tree
```
```
File Management -> Copy Files -> Copy Running Config to Startup Config
```
* Power down the switch, then power it back up after 60 seconds and check the settings have applied.


## Saving switch details
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
* Upgrading the firmware is simple to do via web-based GUI
* Upgrade requires the switch to be configured with an IP address
* Ensure stacked switches are running the same firmware

***
## Hardware support
* Call Dell on 01344-860456 with the service tag for the machine.
* Each switch has its own service tag
* [Use this](http://creativyst.com/Doc/Articles/HT/Dell/DellNumb.htm) to convert tags to express service codes

***
## Known issues
* Configuration download does not result in a usable file; use "show running-config" on the CLI to display current config and save it to a file
* Max length of stacking cable is 3M; there is also a 10M CX4 cable available, but the port must be put into Ethernet mode, not stacking mode
* Max of 8 switches stacked together (although will support up to 12 switches in a stack in some configs)
* Switch can only support 1 x dual-port 10GbaseT copper 10Gb module at once, as this requires more voltage

