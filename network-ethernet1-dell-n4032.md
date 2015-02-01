# Dell PowerConnect N-4000 series 10Gb switches

## Hardware overview
* Four different switches available
   * 24-port switches N4032 (24 x copper 10GbaseT) and N4032F (24 x 10Gb SFP+ ports)
   * 48-port switches N4064 (48 x copper 10GbaseT) and N4064F (48 x 10Gb SFP+ ports)
   * 48-port switches both have 2 x 40Gb QSFP ports onboard (24-port switches don't)
* Switch supports one module available at extra cost; modules provide ONE option of:
   * 4-port 10Gb SFP+
   * 4-port 10Gb 10GbaseT copper RJ45
   * 2-port 40Gb QSFP
* Each 40Gb port can be split into 4 x 10Gb ports, providing up to 8 extra ports
   * This is why 24-port switches are called N4032, and 48-port switches are called N4064
   * Use Dell 40Gb->4x10Gb splitter cables to create 10Gb ports
   * Dell cables provide 10Gb SFP+ ports only
* Dedicated serial and 100baseT console ports, on PSU side of switch
* Ships in most countries by default with dual PSUs
* Front-to-rear airflow (intake through RJ45 ports, exhaust over PSU), with reverse airflow PSUs available
* Supports user-configurable stacking over standard ports (10Gb or 40Gb)
* Does NOT support Dell/Force10 VLT feature (as this switch does not run FTOS); but does support MLAG across stack
* Readyrail kit must be purchased separately; default rack-ears mounting causes brackets to deform
* Switch has standard Dell getting-started wizard, and graphical management web interface

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
 * Rapid spanning-tree (RSTP) enabled
 * Five VLANs configured for different types of data:
    *  VLAN1 = hardware mgt (BMC, switch interface, etc.)
    *  VLAN2 = private network (blue)
    *  VLAN3 = software mgt / build network (PXE boot, ganglia, nagios)
    *  VLAN4 = DMZ network 
    *  VLAN5 = customer external network 

 * To configure the switch, access the command-line interface (via serial console or via telnet)
 * Login using the "admin" account created in basic setup
 * Enter privileged mode, and set the switch hostname ("coresw1" in the example below):
```
enable
configure
hostname "coresw1"
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
## Known issues
* Switch management GUI causes untidy CLI configs to be generated; e.g. changing a port's VLAN mode from GENERAL to ACCESS will leave all the GENERAL configuration lines in the text config, which makes it hard to read
* Switch is heavy and will deform rack ears if installed without ReadyRails kit; these must be ordered separately
* Switch does not support VLT; use MLAG instead with stacking between switches
