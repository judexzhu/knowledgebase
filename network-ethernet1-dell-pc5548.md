# Dell PowerConnect 5548 48-port
## and 5524 24-port switches

## Hardware overview
* 48-port gigabit-Ethernet switch with 2 x SFP+ 10Gb ports and dual front 10Gb HDMI stacking ports
* 24-port version is called PC5524 and also has 2 x 10Gb and 2 x 10Gb HDMI stacking ports
* Single internal PSU, with DC socket for external PSU (sold separately)
* Front-to-rear airflow (intake through RJ45 ports, exhaust over PSU)
* Max stacking cable length of 3M (uses Dell branded HDMI cables, but any cables seem to work)


## Standard Base Configuration
* Connect to the switch via serial console  (9600 baud, 0/n/1)
* Apply power to switch
* Wait for the console to prompt the setup wizard, and press “Y” for yes within 60 seconds
* When prompted to setup SNMP, press No
* When prompted, press Yes to configure IP address, Subnet Mask and Gateway – use as provided by preinstall or otherwise instructed for the cluster
* When prompted, set the username to “admin” and the password to the supplied cluster admin/IPMI password
* Reset the switch as the management interface won't come up until after a full reboot at this point. 
* After the reboot is complete, In your browser connect to the IP address nominated for the switch.
* Apply the following settings via the management GUI:
```
System settings -> General -> System Information -> Set the system name to the name of the switch
```
```
File Management -> Copy Files -> Copy Running Config to Startup Config
```
* Power down the switch, then power it back up after 60 seconds and check the settings have applied.

## **Cluster simple** profile
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
 * Configuration for modern clusters and private clouds
 * Rapid spanning-tree (RSTP) enabled plus default VLANs:
    *  VLAN1 = hardware mgt (BMC, switch interface, etc.)
    *  VLAN2 = private network (blue)
    *  VLAN3 = software mgt / build network (PXE boot, ganglia, nagios)
    *  VLAN4 = DMZ network
    *  VLAN5 = customer external network	

 * To configure, first complete the standard base configuration steps, as shown above
 * Next, connect via telnet to the IP address set in the steps above
 * Enter privileged mode, then enter configuration mode, then set the hostname (here "accsw1"); 
```
enable
configure
hostname "accsw1"
```
 * Create and name all five VLANs
```
interface vlan 1
 name hwmgt
 exit
interface vlan 2
 name private
 exit
interface vlan 3
 name swmgt
 exit
interface vlan 4
 name dmz
 exit
interface vlan 5
 name external
 exit
```
 * Setup all gigabit ports as untagged on VLAN3 (commands shown here are for the 48-port N2048 switch); remember to setup the 10Gb ports as well if you're going to use them 
```
interface range gi1/0/1-48
switchport mode general
switchport general allowed vlan add 3 untagged
switchport general pvid 3
spanning-tree portfast
exit
interface range te1/0/1-2
switchport mode general
switchport general allowed vlan add 3 untagged
switchport general pvid 3
spanning-tree portfast
exit
```
 * For ports connected to servers, add VLAN2; repeat the commands below for all required ports (shown here for port 1)
```
interface gi1/0/1
switchport general allowed vlan add 2 tagged
exit
```
 * For any management servers that need access to VLAN1, 4 or 5, add these VLANs as required; commands shown below adds VLAN1 to port 2
``` 
interface gi1/0/2
switchport general allow vlan add 1 tagged
exit
```
 * Identify the uplink port used to connect to other switches; this port must be configured with standard spanning-tree (NOT portfast), and tagged in all VLANs. This may a 1Gb or a 10Gb port - the configuration below is for the first 10Gb port. 
```
interface te1/0/1
switchport mode general
switchport general allowed vlan add 1-2,4-5 tagged
switchport general allowed vlan add 3 untagged
switchport general pvid 3
exit
```

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