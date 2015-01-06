# Dell PDU

## Standard Base Configuration

* Run a network cable from the PDU into a pooled DHCP network
* Power on the PDU and wait for it to boot
* Once the PDU is booted it will show the load on the LCD screen
* Press and hold the button on the PDU until the LCD displays “IP”
* Note the IP displayed on the LCD screen
* Navigate to IP given in web browser
* Log in with admin / admin
* Apply the following configuration:
```
Administration -> Security -> Local Users -> Administrator -> Change default password
```
```
Administration -> General -> Identification -> Set name
```
```
Administration -> Network -> IPV4 Configuration -> Set to Manual, then assign IP for PDU and set
Subnet Mask and Gateway
```

* Save changes and reboot pdu to test settings have been applied correctly
* Note PDU details / configuration on a generic customer asset sheet
