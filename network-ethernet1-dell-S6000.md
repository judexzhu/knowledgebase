# Dell PowerConnect S6000 40Gb switch

## Hardware overview
* 36-port 40Gb Ethernet 1U switch with dual PSU
* All but 8 of the 40Gb ports can be split into 4 x 10Gb ports, providing 96 x 10Gb and 8 x 40Gb ports in total
* Ships with dual PSU as standard
* Front-to-rear airflow (intake through RJ45 ports, exhaust over PSU), with reverse airflow PSUs available
* Runs FTOS (Force10-OS), which supports virtual-link trunking to improve over standard spanning-tree

## Standard Base Configuration
* Connect to the switch via serial console  (9600 baud, 0/n/1)
* Apply power to switch
* Wait for the switch to boot; takes about 2 minutes
* Configure the hostname ("coresw1" in the example below), disable jumpstart boot, save and reboot the switch
```
enable
configure
hostname coresw1
end
reload-type normal-reload
write mem
reload
```
* Wait for the switch to reboot again
* At login prompt, check that the hostname is now set
* Set a password and enable password, setup management IP on dedicated port, enable syslog logging
```
enable
configure
username admin password ******** priv 15
enable password ********
interface ma 0/0
ip address 10.111.0.100 255.255.0.0
no shutdown
exit
logging 10.111.254.1
end
write mem
```
* You may want to convert some 40Gb ports into 10Gb ports. You can do this to any ports except the 4 top-right and 4 top-left ports, which must always be 40Gb (4,12,20,28,100,108,116,124). 
* Enter the following command to switch a 40Gb port into 4 x 10Gb ports (configuring port 16 in the example below)
```
enable
configure
stack-unit 0 port 16 portmode quad
end
write mem
```
* You will need to reboot the switch (use the `reload` command) to change a 40Gb port to 4x10Gb
* Any existing port config is lost when you change modes; when the switch next boots, you'll see error messages when loading the config, which you can ignore. These errors won't be shown the next time the switch boots. 
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
* It can be difficult to confirm that VLT is actually running and doing what it's supposed to be doing; test your configuration before the system ships
* VLT *requires* a completely isolated heartbeat 

