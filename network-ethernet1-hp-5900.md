# HP Networking 5900 series 
## 24 and 48-port 10Gb switches with 40Gb ports

## Hardware overview
* 48-port 10Gb switch version with 4 x QSFP 40Gb ports
* 24-port 10Gb switch version with 2 x QSFP 40Gb ports
* Ships with ZERO PSUs by default; has two PSU slots
* 10Gb ports support DAC twinax copper cables (but will not negotiate to 1Gb using these)
* Supports 1Gb RJ45 SFP+ modules (ordered separately)
* Dedicated 1Gb management port (or supports adding management IP to any VLAN)
* HP has their own configuration language which is different to Cisco, Force10, Juniper, etc.

## Standard Base Configuration
* Connect to the switch via serial console  (9600 baud, 0/n/1)
* Apply power to switch
* Wait for console prompt
* Enter system configuration mode by typing "system"
* Set the hostname:
```
sysname coresw1
```
* Set default VLANs:
```
vlan 1
 description hwmgt
quit
vlan 2
 description private
quit
vlan 3
 description swmgt
quit
vlan 4
 description dmz
quit
vlan 5
 description external
quit
```
* Set an IP address for the management interface, or VLAN1:
```
interface M-GigabitEthernet0/0/0
ip address 10.111.0.101 255.255.0.0
quit

--or--

interface vlan1
ip address 10.111.0.101 255.255.0.0
quit
```
 * Set login passwords for console and telnet
```
line aux 0
 authentication-mode password
 user-role network-admin
 set authentication password simple mypassword123
 quit
line vty 0 4
 user-role network-admin
 user-role network-operator
 set authentication password simple mypassword123
 protocol inbound telnet
 quit
```
* Configure the local admin user
```
user-group system
 local-user admin class manage
  password simple mypassword 123 
  service-type telnet
  authorization-attribute user-role network-admin
  authorization-attribute user-role network-operator
  quit
```
* Setup any trunk ports (e.g. all four 40Gb ports in this example)
```
interface range FortyGigE 1/0/49 to FortyGigE 1/0/52
 port link-mode bridge
 port link-type trunk
 port trunk permit vlan 1 to 5
 quit
```
* Setup standard host ports (e.g. all 10Gb ports in this example)
```
interface range Ten-GigabitEthernet 1/0/1 to Ten-GigabitEthernet 1/0/48
 port link-mode bridge
 port link-type hybrid
 port hybrid vlan 1 to 2 4 tagged
 port hybrid vlan 3 untagged
 port hybrid pvid vlan 3
 stp edged-port
 quit
```
* **SAVE the configuration**
```
save
```
* Power cycle the system and check that settings are saved

## Saving switch details
* Use the "display diagnostic" command to show system serial number
* Use "display version" to show firmware version
* Use "display current" to show current configuration

***

## Known issues
* Configuration language is confusing; show=display, exit=quit
* Remember to type "save" to store any configuration changes made
* When you login, you're in user-mode by default; type "system" to enter configuration mode