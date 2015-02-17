# HP C7000 Blade Enclosure

## Resetting OA Password

If the OA password has been lost or forgotten, the password can be reset using the following method:

* Using your workstation or laptop, create a file named `reset_password.cfg` and insert the following contents - with the desired username and password, then save to a USB key

`SET USER PASSWORD "admin" "adminpass"`

* Plug the USB key into the active OA USB slot
* Using the chassis screen, select `USB Key Menu`, then click OK
* Select `Restore Configuration`, then select `reset_password.cfg`
* Confirm the operation, then wait for the OA to reboot. Once the OA has been rebooted, the new username and password will be active.

## Restoring to factory defaults

The following steps can be used to restore the HP C7000 Chassis and its blade servers to factory defaults.

* Insert all power cables and power on
* Wait for the chassis and all of the blades to boot
* Using the chassis screen, navigate to `Enclosure Settings` and set OA1 to:

**IP**: (example) 10.11.0.5

**Subnet Mask**: (example) 255.255.0.0

**Gateway**: (example) 10.11.0.1

* Insert Ethernet cable into OA iLO port, then connect to workstation/laptop etc
* Connect to the OA using `telnet 10.11.0.5`
* Log in with the previously set or default administrator username and password
* The chassis and its blades can be reset with the command

```SET FACTORY```

* Answer `YES`
* The process will take approximately 4 minutes
* Once the chassis has reset, the chassis is ready for configuration via the GUI at https://10.11.0.5 etc.