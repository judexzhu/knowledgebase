# Dell PowerConnect S4032F 10Gb switch

## Hardware overview
* 24-port 10Gb Ethernet 1U switch with dual PSU
* Each 40Gb port can be split into 4 x 10Gb ports, providing 144 x 10Gb ports in total
* Available with dual PSU
* Front-to-rear airflow (intake through RJ45 ports, exhaust over PSU), with reverse airflow PSUs available

## Standard Base Configuration (all profiles)
* Connect to the switch via serial console  (9600 baud, 8/n/1)
* Apply power to switch
* Wait for the console to prompt the setup wizard, and press “Y” for yes within 60 seconds
* When prompted to setup SNMP, press No
* When prompted, press Yes to configure the out of band IP address, Subnet Mask and Gateway – use as provided by preinstall or otherwise instructed for the cluster
* When prompted, press No to configure the VLAN1 routing interface
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
Switching -> Spanning Tree -> Global Settings -> Disable Spanning Tree
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
Switching -> Spanning Tree -> Global Settings -> Enable Rapid Spanning Tree
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
![draft](http://upload.wikimedia.org/wikipedia/commons/f/ff/DRAFT_ICON.png)
* Switches usually ship on latest firmware. 
* Upgrade the firmware via CLI
* Upgrade requires the switch to be configured with an IP address
* Ensure stacked switches are running the same firmware

***
## Hardware support
* Call Dell on 01344-860456 with the service tag for the machine.
* Each switch has its own service tag, printed on rear side (by PSUs)
* [Use this](http://creativyst.com/Doc/Articles/HT/Dell/DellNumb.htm) to convert tags to express service codes
