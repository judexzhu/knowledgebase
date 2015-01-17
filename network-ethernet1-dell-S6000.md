# Dell PowerConnect S6000 40Gb switch

## Hardware overview
* 36-port 40Gb Ethernet 1U switch with dual PSU
* Each 40Gb port can be split into 4 x 10Gb ports, providing 144 x 10Gb ports in total
* Ships with dual PSU as standard
* Front-to-rear airflow (intake through RJ45 ports, exhaust over PSU), with reverse airflow PSUs available
* Runs FTOS (Force10-OS), which supports virtual-link trunking to improve over standard spanning-tree

## Standard Base Configuration
![draft](http://upload.wikimedia.org/wikipedia/commons/f/ff/DRAFT_ICON.png)
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
Switching -> Spanning Tree -> General Settings -> Disable Spanning Tree
```
```
File Management -> Copy Files -> Copy Running Config to Startup Config
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

***
## Cable support
* Use officially branded Dell 40Gb QSFP cables only
* Also works with Mellanox and Intel copper and optical cables (tested up to 30M)
* 40Gb->4x10Gb breakout cables come in 2M and 5M lengths (copper) and 7M (optical)
* Optical break-out cables require both 40Gb and 10Gb optical transceivers 

***
## Known issues
* Only available models have port->PSU airflow, meaning that RJ45 ports should face the front of the rack 

