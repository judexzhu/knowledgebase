#HP MSA 2040 Setup Guide
##Scratch profile
* Navigate to the URL of the controller array
* Enter the default username and password
* Using the top navigation bar, click `System: <hostname> -> Set System Information`

Field | Information required
:---: | ---
System Name | Hostname, e.g. `hablkary1-1`
System Contact | `Alces Software`
System Location | Rack name e.g. `rackA1`
System Information `hpccluster - scratch storage`

* Navigate to `System -> Rear` - note down all chassis and controller serial numbers
* Navigate to `Pools`
  * `Action -> Add Disk Group`

Per disk array, create 1 disk group with the following settings: 

Option | Setting
:---: | :---: 
`Type` | Linear
`Name` | `<scratch/tierX>-ostX`
`RAID Level` | `RAID-6`
`Chunk size` | `128KB`
`Online Initialisation` | Yes

Add disks 1-10 into the `RAID6` pool, and optionally select the hot-spare disk into the `SPARE` pool. 
Repeat for each disk array in the group. 

* Navigate to `Volumes`
  * `Action -> Create Linear Volumes`

For each disk pool, create a linear volume with the following settings: 

Option | Setting
:---: | :---:
`Pool` | Disk pool, e.g. `tier1-ost1`
`Number of volumes` | `1`
`Volume name` | Default provided, e.g. `tier1-ost1_v0001`
`Volume size` | Default provided
`Enable snapshots` | Disabled

Repeat for each disk pool created

