# Dell PowerConnect 2848 48-port
## and 2824 24-port switches

## Hardware overview
* 48-port gigabit-Ethernet switch 
* 24-port version is called 2824 
* Single internal PSU with Side-to-side airflow

## Standard Base Configuration
* These switches have a "MANAGED" and an "UNMANAGED" mode
* Switches usually ship in UNMANAGED mode 
* If the switch has already been configured, follow these steps to reset it:
    * Press and hold the “MANAGED” switch for between 3 and 8 secs to switch between managed / unmanaged mode 
    * If the switch does not show the setup wizard, press and hold the button for more than 8 seconds to reset it to defaults
    * If the switch is stuck in managed mode and you can login (admin, no password) and type:
```
enable
restore defaults
reload 
```

* If the switch is brand new and unconfigured, connect to the switch with serial port 
* Configure the console at 9600 baud, 0/n/1; type:
```
>enable
>reload
```
* Wait for the switch to reboot, then when prompted press yes within 60 seconds to enter the setup wizard 
    * When prompted to setup SNMP, select No. Follow the remainder of the wizard
    * Set username to “admin” and password to supplied admin/IPMI password for cluster.
    * Enter the netmask, IP address & gateway as provided from pre-install
    * Log into the switches GUI on your web browser with the nominated IP address. 

* In your browser connect to the IP address nominated for the switch.
* Apply the following settings via the management GUI:
```
Switching -> Spanning Tree -> General Settings -> Disable Spanning Tree
```
* Click the save button in the GUI (icon looks like a floppy-disk), and log out of the GUI
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
* Upgrading the firmware is simple to do via web-based GUI
* Upgrade requires the switch to be configured with an IP address

***
## Hardware support
* Call Dell on 01344-860456 with the service tag for the machine.
* Each switch has its own service tag
* [Use this](http://creativyst.com/Doc/Articles/HT/Dell/DellNumb.htm) to convert tags to express service codes

***
## Known issues
* Pressing the "manage" may reset all managed switch settings when switch is returned to unmanaged mode. If you had VLANs and spanning-tree configured, this could cause other network switches to fail if they are not expecting your switch to return to default settings. 
* The switch does not support redundant PSUs; they can however be deployed with APC automatic transfer switches to power them from two separate supplies
* The switch management GUI makes it difficult to tell when settings have been saved
* The CLI/console provides very few useful commands; the command "show tech-support" can be used on the command-line to show the default configuration, and serial numbers