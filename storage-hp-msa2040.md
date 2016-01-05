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
2. Disk drive firmware can (sometimes) be upgraded via the GUI. However - the GUI process can be unreliable, so there is another method using FTP. Follow the steps below to upgrade disk drive firmware via the command-line interface:

  * Get your disk firmware file - these are downloadable from HP and usually end in a ".fla" extension. Double check the MD5sum of your file matches the checksum given on HP's website. 
  * Copy the firmware file to a location that can access the array's controllers over the network. Make sure you have an FTP client installed on that system. 
  * Login to the disk array via FTP using the "manage" username and password
```
ftp blockary1-1c1
Connected to blockary1-1c1 (10.111.0.171).
220-Welcome to Pure-FTPd.
220-You are user number 1 of 5 allowed.
220-Local time is now 16:42. Server port: 21.
220-This is a private system - No anonymous login
220 You will be disconnected after 15 minutes of inactivity.
Name (blockary1-1c1:root): manage
331 User manage OK. Password required
Password:
230-OK. Current restricted directory is /
```
  * Use the "put" command to put your firmware file onto a disk; e.g. if you want to upgrade disks in slots 11 and 12 of enclosure zero, you'd enter:
```
ftp> put HP_HUH728080AL5204_C7J0.fla disk:1.11-12
```
  * Alternatively, to upgrade all target disks that will accept the new firmware, just use:
```
ftp> put HP_HUH728080AL5204_C7J0.fla disk
```

***
## Known issues
 * The array cannot create a RAID10 disk group larger than 16 disks. 
 * There are limits on how many physical disks the array can support; check HP documents for details
 * The disk array does not support copy-back (i.e. when a RAID disk fails, the array uses a hot-spare and will not copy the data back to the drive in the original slot location when the broken disk is replaced)
 * Newly inserted disks (in any slot) are not automatically assigned as spares
 * If a disk is removed and reinserted (or temporarily fails and revives itself again), the array detects that it already has data on and will not use it. Select "clear metadata" from the array GUI main menu to wipe the disk and allow it to be used. 
 * Remove SNMP probing via Nagios is unreliable if probed too often
 * Known issues with 8TB disks on firmware revision C515; upgrade them to version C7J0 or later before entering production