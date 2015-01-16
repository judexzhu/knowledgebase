# Dell PowerConnect N2048 48-port
## and N2024 24-port switches

## Hardware overview
* 48-port gigabit-Ethernet switch with 2 x SFP+ 10Gb ports and dual rear 21Gb stacking ports
* 24-port version is called N2024 and also has 2 x 10Gb and 2 x 21Gb stacking ports
* Single internal PSU, with DC socket for external PSU (sold separately)
* Front-to-rear airflow (intake through RJ45 ports, exhaust over PSU)
* Max stacking cable length of 3M (uses SFF8088 aka. mini-SAS or 6Gb-SAS connectors)

## Standard Base Configuration

# Check config settings **

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
* Switches usually ship on latest firmware. 
* Upgrade the firmware via web-based GUI
* Upgrade requires the switch to be configured with an IP address
* Ensure stacked switches are running the same firmware

***
## Hardware support
* Call Dell on 01344-860456 with the service tag for the machine.
* Each switch has its own service tag
* [Use this](http://creativyst.com/Doc/Articles/HT/Dell/DellNumb.htm) to convert tags to express service codes

***
## Known issues
* Only available models have port->PSU airflow, meaning that RJ45 ports should face the front of the rack 

