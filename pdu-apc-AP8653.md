# APC AP8653 switched 0U single-phase PDU

## Standard Base Configuration

* Run a network cable from the PDU into a pooled DHCP network
* Power on the PDU and wait for it to boot
* Once the PDU is booted it will show the load on the LCD screen
* Press and hold the button on the PDU until the LCD displays “IP”
* Note the IP displayed on the LCD screen
* Navigate to IP given in web browser
* Log in with default factory user apc / apc
* Apply the following configuration:
```
Browse to: Configuration -> Security -> Local Users -> Management
```
``` 
Add new user: admin / clusteradminpass, select "Administrator" security level
```
```
Select 'apc' SuperUser and set password to cluster admin password
```
```
After ensuring that the newly created 'admin' account is accessible with the correct privileges, disable the 'apc' user account - then disable 'device' and 'readonly' accounts.
```
```
Configuration -> TCP/IP -> IPV4 Configuration -> Set to Manual mode and assign IP for PDU and set Subnet Mask and Gateway
```
```
Configuration -> General -> Identification -> Set correct name and location for PDU (e.g. pduB1 / rackA)
```
```
Configuration -> General -> Date and Time -> Mode -> Set date and timezone to correct location and set time.
```

* Plug desired network cable back into PDU, reset PDU and check settings have been applied correctly
* Note PDU details / configuration on a generic customer asset sheet
