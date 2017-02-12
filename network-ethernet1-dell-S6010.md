# Dell PowerConnect S6000 40Gb switch

## Hardware overview
* 36-port 40Gb Ethernet 1U switch with dual PSU
* All but 8 of the 40Gb ports can be split into 4 x 10Gb ports, providing 96 x 10Gb and 8 x 40Gb ports in total
* Ships with dual PSU as standard
* Front-to-rear airflow (intake through RJ45 ports, exhaust over PSU), with reverse airflow PSUs/fans available
* Runs FTOS (Force10-OS), which supports virtual-link trunking to improve over standard spanning-tree

## Standard Base Configuration
* Connect to the switch via serial console  (115200 baud, 0/n/1); serial cable comes with switch
* Apply power to switch
* Wait for the switch to boot; takes about 2 minutes
* Abort BMP (config file from network boot) by pressing "A" on start-up
* Press return a few times to get a prompt; enter the following commands to configure the switch
```
enable
configure
reload-type
boot-type normal-reload
end
write
reload
```
Switch will reboot with a static config; once complete, press return again to get a prompt and enter configuration commands below:

```
enable
config
hostname <switch name>
username admin password ******** priv 15
enable password ********
protocol spanning-tree rstp
no disable
ex
interface ManagementEthernet 1/1
ip address 10.111.0.91 255.255.0.0
no shutdown
exit
```

The S6010 switch ships with all ports configured as 40Gb by default. 
 - Individual 40Gb ports can be changed to 4 x 10Gb instead 
 - Change happens immediately (i.e. without the reboot needed on S6000)
 - cannot modify ports 1,3,5,7 and 25,27,29,31 which must stay as 40Gb

To make quad (4x10Gb) ports, enter the following command for each port to change (changing port 6 in the example below):
```
stack-unit 1 port 6 portmode quad
```

* Remember to save changes using "end" and "write" commands after making changes
* Any existing port config is lost when you change modes; when the switch next boots, you'll see error messages when loading the config, which you can ignore. These errors won't be shown the next time the switch boots. 
* Power down the switch, then power it back up after 60 seconds and check the settings have applied.

### Saving switch details

Use the following commands to get information about the switch for asset sheets:
```
show version
show inventory
```

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
* Port naming for is different to the (similar looking) Dell S6000 switch:
   - On S6000, ports were numbered Fo0/0, Fo0/4, Fo0/8 (etc.) for 40Gb, and Te0/12, Te0/13, Te0/14, Te0/15 for a quad port
   - On S6010, ports are numbered Fo0/1, Fo0/2, Fo0/3 (etc.) and Te0/4/1, Te0/4/2, Te0/4/3, Te0/4/4 for a quad port
* It can be difficult to confirm that VLT is actually running and doing what it's supposed to be doing; test your configuration before the system ships
* VLT *requires* a completely isolated heartbeat network
* 8 of the 40Gb ports CANNOT be split into 4x10Gb ports; they are ports 1,3,5,7 and 25,27,29,31. The switch hosts a maximum of 96 x 10Gb ports and 8 x 40Gb ports. 
