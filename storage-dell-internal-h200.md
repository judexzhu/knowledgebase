# Dell PERC H200
## and 6Gbps SAS HBAs

## Hardware Overview
 * SAS HBA card with customised Dell firmware
 * Internal cards have two 4x 6Gb SAS ports (24Gb per physical socket)
 * External cards have two 4x 6Gb SAS ports (aka. miniSAS-26 or SFF8088 connectors) ![SFF8088](http://www.cs-electronics.com/images-large/SFF-8088-L-new.jpg)
 * LSI SAS 2008 chipset; standard LSI drivers and management tools can be used
    * H200 (internal) card usually ships with LSI-IR (integrated RAID) firmware, supporting RAID 0/1/10
    * HBA (external) card usually ships with LSI-IT (initiator/target) firmware, which doesn't support RAID
    * Both firmware revisions support booting via INT19 BIOS hook (i.e. legacy BIOS boot, not UEFI compatible)
    * Dell cards will accept Dell or standard LSI firmware (which will invalidate warranty)


## RAID configuration
![draft](http://upload.wikimedia.org/wikipedia/commons/f/ff/DRAFT_ICON.png)

1. Press CTRL+C on boot to configure the card
2. Setup RAID type, boot volume, etc. for the card
3. Exit and press CTRL+ALT+DEL to reboot

***
### Upgrading firmware

1. Firmware can be downloaded to card using DOS boot environment, or from Linux
2. Cards usually ship on latest firmware, and rarely need updating
3. Download new firmware from Dell support site. 
4. Upgrading firmware writes a new image to the SAS card; reboot the server to apply the new version
5. In very rare circumstances, a new RAID firmware may not support an existing RAID group. Backup data before upgrading firmware. 
6. If using the Linux update binaries, execute the update package on the server. The command line switches `-f -n -q` will cause the update packages to run automatically without prompting or rebooting. Firmware update from Linux takes around 1 minute to apply. 
7. RAID configurations are written to disks and detected automatically as the server POSTs - RAID card does not store the disk configuration in non-volatile memory. 

***
## Hardware support
 * Dell RAID cards take on the support coverage of the server they're installed in. Open calls for the containing server

***
## Known issues
 * RAID1 performance is typically poor as card only supports direct-I/O (no RAM on SAS card), particularly with slow disks
 * Dell only support connecting to Dell storage devices (although LSI hardware usually works well with most SAS devices, including SAS switches)