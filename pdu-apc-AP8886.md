# APC AP8886 monitored 0U three-phase 32A PDU

* Zero-U PDU with no large protrusions
* 22KW max output (2 x 16A banks on each of 3 phases)
* Total of 30 x C13 and 12 x C19 outputs split per phase
* Red 32A 3-phase commando plug on 3M cable

## Standard Base Configuration

* Connect the serial cable that came with the PDU to your serial port
* Setup your terminal for 9600/8/n/0 no flow-control
* Power on the PDU and wait for it to boot (15-20 secs)
* Once the PDU is booted it will show the load on the LCD screen
* Press return a few times to get a login prompt
* Login with username/password (apc/apc)
* Configure IP address, hostname and password

```
tcpip  -i 10.111.0.97 -s 255.255.0.0 -h pdu1a
user -n admin -pw ********* -pe Administrator -e enable
user -n apc -pw *********
system -n pdu1a
reboot
```

* Wait for the PDU to reboot
* Login again (using the new password)
* Check the settings have been saved:
```
tcpip
```

### Getting system information

* Login to the serial console, and use the "about" command
```
about
```


## Known issues

* You can drawer a maximum of 16A per bank; there are six banks in total per PDU. 
* Exceeding 16A will trip the breaker on the bank; PDU must be visited to reset the trip
* Power Sockets are not labelled on the PDU
* Default orientation (power cable up) can be reversed, but writing on PDU will be upside down