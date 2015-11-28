# HP MSA2040 SAS array

## Hardware Overview
 * 12Gb SAS controller; each has 4 x 12Gb SAS ports and 1 x 6Gb expansion port
 * 6Gb SAS expander; each has 1 x 6Gb SAS-IN and 1 x 6Gb SAS-OUT
 * 2U enclosure takes one or two controllers or two SAS expanders (mixing types not supported)
 * 2U enclosure either has 24 x 2.5" disk slots or 12 x 3.5" disk slots

## RAID configuration
 * Array supports Linear arrays of RAID10 (up to 16 disks) or RAID6 (up to 16 disks)
 * Dedicated and global hot-spares supported

***
### Configuring the array
1. Install array on rack-rails and cable
2. Power on the system and leave it for 5-10 mins for DHCP to timeout
3. Setup a laptop with IP address 10.0.0.1 / 255.255.255.0
4. Browse to https://10.0.0.2 (MUST be https not http) - tested with FireFox and Chrome on Windows
5. Login with username "manage" and password "!manage"
6. Click on the version/hostname box in GUI (top, centre) - set system information; set hostname and owner details
7. Click version/hostname box again - Select System services menu and enable SNMP
8. Click on the username box (top, right) and set the password for the "manage" user
9. On the "Pools" tab, select "create pool" from the menu and setup linear disk pools (RAID groups)
10. On the "Volumes" tab, select "create volume" from the menu and setup one volume per pool
11. On the home menu, click "setup global hot-spares" from the menu; add hot-spare disks
12. Mouse-over the diagram of the enclosures on the home menu (near where the tray ID is displayed) to show serial number
13. From the home tab, select the "setup wizard" ACTION menu and set IP addresses only
14. Modify the IP address on your laptop to match the customer network
15. Reconnect to the array on new IP to confirm settings

### Upgrading firmware

1. Firmware can be uploaded using the web GUI. Click on the button at the top left of the main HTTPS interface and follow the prompts. 

***
## Known issues
 * The array cannot create a RAID10 disk group larger than 16 disks. 
 * There are limits on how many physical disks the array can support; check HP documents for details
 * The disk array does not support copy-back (i.e. when a RAID disk fails, the array uses a hot-spare and will not copy the data back to the drive in the original slot location when the broken disk is replaced)
 * Newly inserted disks (in any slot) are not automatically assigned as spares
 * If a disk is removed and reinserted (or temporarily fails and revives itself again), the array detects that it already has data on and will not use it. Select "clear metadata" from the array GUI main menu to wipe the disk and allow it to be used. 
 * Remove SNMP probing via Nagios is unreliable if probed too often