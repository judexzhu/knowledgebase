# Dell PowerConnect 3548

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